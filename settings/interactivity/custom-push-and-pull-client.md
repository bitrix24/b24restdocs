# Custom Push&Pull Client

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A custom client keeps a connection to the Push&Pull servers directly: the application changes its state and updates the interface immediately, without polling REST.

The exchange works as follows. The server side of the application publishes an event with the [pull.application.event.add](./pull-application-event-add.md) method, the event lands in the application channel on the Push&Pull servers, and the client reads the channel over an open connection. The client connects to both application channels at once. The definitions of the terms of this section and the lifetimes of a channel and of the configuration are collected in the [{#T}](./index.md) article.

A custom client is needed when the application runs outside the Bitrix24 interface: a server-side integration process, a desktop application, or a web application on your own domain outside the frame. For example, a job on your server spends several minutes parsing an export and reports the completion to the client, which shows the result right away without polling the server.

In other cases, the solutions from the neighbouring articles will do:

- the application is open in Bitrix24 and the built-in client is enough — [{#T}](./push-and-pull-in-browser.md)
- events are needed while the application page is closed and Bitrix24 is open in the browser — [PAGE_BACKGROUND_WORKER](../../api-reference/widgets/universal/background-worker.md)
- the user has to be notified outside the Bitrix24 interface — [pull.application.push.add](./pull-application-push-add.md)

{% note info "" %}

The client works only in the context of an [application](../app-installation/index.md). It requests the connection configuration with the [pull.application.config.get](./pull-application-config-get.md) method, which requires an OAuth token and the `pull` scope, and a webhook does not create such a context.

{% endnote %}

## Before You Start

- an installed [application](../app-installation/index.md) with the [`pull`](../../api-reference/scopes/permissions.md) scope
- a server side that can call REST with the application OAuth token

The connection configuration is requested by the server side of the application — the OAuth token stays there. Pass only the server address and the connection parameters from the response to the client.

{% note warning "" %}

`CHANNEL_ID`, `jwt`, and `clientId` grant access to the channel. Whoever obtains them reads the application events until the channel expires. Do not write them to logs, do not retain them in open storages, and do not pass them to third parties.

{% endnote %}

## How to Connect to the Server {#connect}

The server supports two connection types: websocket and long polling. Connect over websocket by default. Long polling is needed only for devices without websocket support or when the connection drops regularly.

1. Retrieve the connection parameters with the [pull.application.config.get](./pull-application-config-get.md) method
2. Check `server.server_enabled`. The `false` value means that Push&Pull is not configured in this Bitrix24: there is nothing to connect to, and reconnecting will not help
3. Take the secure address of the required type from the `server` object — `websocket_secure` or `long_pooling_secure`. The connection parameters are passed in the query string, so use the insecure `websocket` and `long_polling` only where there is no secure address. If `server.websocket_enabled` equals `false`, websocket is disabled — connect over long polling
4. Choose how to authorize the connection. The `jwt` and `clientId` fields never arrive together, so there are only two branches:
    - the response contains the `jwt` field — pass it in the `token` GET parameter. The channels are already embedded in the token, and `CHANNEL_ID` is not needed. The token is issued only by a server of version 5 or higher, so this branch does not occur in Bitrix24 cloud
    - there is no `jwt` field — build the `CHANNEL_ID` GET parameter from the `channels.private.id` and `channels.shared.id` values, in exactly that order and separated by `/`
5. If the response contains the `clientId` field, add it to the address as a separate parameter

The `CHANNEL_ID` value can be substituted as is or encoded together with the whole query string — the server accepts both options. When encoded, the `/` separator turns into `%2F` and remains a separator.

Do not split the channel IDs into parts: the `id` value of the personal channel itself contains a colon, and that colon is not a channel separator.

The `public_id` public identifiers and the `publicChannels` object are not used for the connection in either branch. The composition of the `server` object is described in the [Server Object](./pull-application-config-get.md#result-server-type) section, the channel fields are in the [Shared and Private Channel Object](./pull-application-config-get.md#result-channel-type) section, and the conditions under which `jwt` and `clientId` arrive are in the [Result Object](./pull-application-config-get.md#result) section.

An example of a websocket connection to the shared Push&Pull server:

```text
wss://rtc-cloud-ms1.bitrix.info/subws/?CHANNEL_ID=beb502091dfc9b93d7fd648aa4ec332e%3A7cc478c89de71ec78bf4820d3d814a3e.4f5466742ca1e59e263fee732a7dbe002889ba91%2F1ab4f7a440cea35a1abccd5c2566c688.b33914ef342e5cd21e4fbcf4ac92acd2e9ea3755&clientId=fcda45d0859442735f07b8bb5825ded1&format=json
```

An example of a websocket connection to a self-hosted Push&Pull server. The address and the path are set by the administrator when the server is configured, so they differ from the addresses of the shared server:

```text
wss://rt.**put.your-domain-here**/sub/?CHANNEL_ID=46a437d2336d4a88e4e9b3cd956ecf45:6221e0eb48981fce67cf4756e82e8102.7910bb25e660bf211fdec15e33c5e25e4c3b644a/fb9f7e13dc3d595c5aefe1a0216c27a2.2887eebc6ae160713a732893462dce9d8e23a7b0
```

In the first example the address is written with percent encoding. There is no `format` parameter in the second example: on a self-hosted server the default version is 2.

An example of a connection with a token, when the method response contains the `jwt` field:

```text
wss://rt.**put.your-domain-here**/sub/?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.**put_jwt_here**&format=json
```

A connection address can contain up to five GET parameters:

- `CHANNEL_ID` or `token` — connection authorization, one of the two is required
- `clientId` — only on the shared Push&Pull server
- `format` — the command format
- `mid` or the `tag` and `time` pair — reading the channel history

The history parameters are taken from the last command received. Having connected with them, the client receives once the messages it missed while it was disconnected.

Request the configuration again when the `end` time of one of the channels or the `exp` time of the configuration itself is approaching. In the self-hosted version `exp` does not always arrive — if there is no such field, rely on the `end` of the channels and on the `config_expire` and `server_restart` commands.

### How an Open Connection Behaves

To read the application events, you do not need to send anything to the socket after connecting: the exchange is one-way, and the commands go from the server to the client.

The sign of a live connection is messages from the server: besides events it regularly sends service ones. The built-in Bitrix24 client considers a connection stalled if it has not received a single message for 20 seconds — it closes the socket and reconnects. Keep the same time rule in your client: treat a long silence as a drop, not as a lull.

### Connecting over Long Polling

Long polling works with the same GET parameters as websocket — only the base address changes, and it is taken from `server` by the rule of step 3. The `long_pooling_secure` field name arrives from the API with a typo, `pooling` instead of `polling`; read it as is.

The client keeps one open GET request and reopens it after each response:

1. Send a GET request to the address you have built
2. You received `200` — parse the response body as commands and send the next request immediately
3. You received `304` — there were no commands, send the next request immediately
4. You received any other status — treat it as a connection error and wait out the delay from the [Error Handling](#errors) section
5. There is no response for more than 60 seconds — abort the request yourself and open a new one

The built-in Bitrix24 client waits 60 seconds for a response and does not open the next request until the previous one has finished. Keep the same rule — otherwise a queue of parallel requests will go to the channel.

## General Format of Commands from the Server

The application events can be read in the text format or in JSON — that is enough. The built-in Bitrix24 client supports two more modes, JSON-RPC and binary, but it needs them for the service exchange rather than for reading the channel.

The format depends on the server version — it arrives in the `server.version` field of the response of the [pull.application.config.get](./pull-application-config-get.md) method. On the shared Push&Pull server the version is always 4.

For a server of version 4 or higher, add the `&format=json` GET parameter when connecting — the commands will arrive in JSON:

```json
[
    {"id":320146,"mid":"14526134350000000000320146","channel":"6221e0eb48981fce67cf4756e82e8102","tag":"672","time":"Thu, 29 Jun 2017 09:50:16 GMT","text":{},"extra":{}},
    {"id":320147,"mid":"14526134350000000000320147","channel":"6221e0eb48981fce67cf4756e82e8102","tag":"673","time":"Thu, 29 Jun 2017 09:50:17 GMT","text":{},"extra":{}}
]
```

For a server of version 3 or lower, the commands arrive as text of the following form:

```text
#!NGINXNMS!#{"id":320146,"mid":"14526134350000000000320146","channel":"6221e0eb48981fce67cf4756e82e8102","tag":"672","time":"Thu, 29 Jun 2017 09:50:16 GMT","text":{},"extra":{}}#!NGINXNME!#
#!NGINXNMS!#{"id":320147,"mid":"14526134350000000000320147","channel":"6221e0eb48981fce67cf4756e82e8102","tag":"673","time":"Thu, 29 Jun 2017 09:50:17 GMT","text":{},"extra":{}}#!NGINXNME!#
```

To parse such a command, take the text between the `#!NGINXNMS!#` and `#!NGINXNME!#` markers and convert it into JSON.

All commands arrive in the channel, including the service commands of the `pull` module, so filter them yourself by the `text.module_id` and `text.command` fields.

The command itself has the same form in both formats:

```json
{
    "id": 320146,
    "mid": "14526134350000000000320146",
    "channel": "6221e0eb48981fce67cf4756e82e8102",
    "tag": "672",
    "time": "Mon, 03 Oct 2017 06:36:01 GMT",
    "text": {
        "module_id": "application",
        "command": "test_event",
        "params": {
            "grid_id": 15,
            "status": "done"
        }
    },
    "extra": {
        "server_time": "2017-10-03T08:36:01+02:00",
        "server_time_unix": 1507012561,
        "server_time_ago": 0,
        "server_name": "rt1.bitrix24.com",
        "revision_web": 19,
        "revision_mobile": 3,
        "channel": "6221e0eb48981fce67cf4756e82e8102"
    }
}
```

where:

- `id` — the message identifier
- `mid` — the message identifier for restoring the history, only for server version 3 and higher
- `channel` — the channel identifier, only for server version 3 and higher. For server version 1 the identifier arrives in `extra.channel`, and for version 2 it arrives in neither place — take it from the value you substituted into `CHANNEL_ID` yourself
- `tag` — the E-tag for restoring the history, for server version 2 and lower
- `time` — the message time for restoring the history, for server version 2 and lower
- `text` — the structure describing the action of the command:
    - `module_id` — the identifier of the module that sent the command. For application events this is the value of the `MODULE_ID` parameter of the [pull.application.event.add](./pull-application-event-add.md) method, `application` by default
    - `command` — the command identifier
    - `params` — additional data for executing the command
- `extra` — the structure with additional details:
    - `server_time` — the server time at the moment the command was formed, in the ATOM format
    - `server_time_unix` — the server time at the moment the command was formed, as a Unix timestamp with fractions of a second
    - `server_time_ago` — the number of seconds elapsed since the command was sent. The server does not send this field; the client substitutes it, having calculated it from `server_time_unix`
    - `server_name` — the name of the server that sent the command
    - `revision_web` — the revision of the Push&Pull protocol for the browser client
    - `revision_mobile` — the revision of the Push&Pull protocol for the mobile client
    - `channel` — the channel identifier for server version 1, see the description of the `channel` field above

## How to Verify the Connection

Send an event from the server side with the [pull.application.event.add](./pull-application-event-add.md) method, with `COMMAND` equal to `test_event`. A command whose `text.module_id` equals `application` and whose `text.command` equals `test_event` has to arrive in the channel.

If the command has not arrived, check the following in order:

- the connection is established and did not close right after it was opened
- `CHANNEL_ID` is built by the rule from the [How to Connect to the Server](#connect) section and did not lose any parts while the address was being encoded
- the channels have not expired — compare the current time with `end` from the response of [pull.application.config.get](./pull-application-config-get.md)
- the event was sent with the same `USER_ID` whose channel the client reads, or without `USER_ID` — in that case it goes to the common channel

## Error Handling {#errors}

Handling connection errors is mandatory: the server will block a client that reconnects without a delay for suspicious activity. The specific intervals below are a reference point, not a requirement of the protocol.

If connecting to the server results in errors, increase the delay before the next attempt. The built-in Bitrix24 client waits out the following delays:

- a drop of an already established connection — 0.5 seconds
- after the first and the second failed attempt — 5 seconds
- after the third and the fourth — 25 seconds
- from the fifth to the ninth — 45 seconds
- starting with the tenth — 60 seconds

Add a random increment of 0 to 20% to the delay so that clients do not reconnect simultaneously. Reset the counter of failed attempts as soon as the connection is established.

If websocket does not come up for several attempts in a row and the connection closes with code `1006` or `1008`, the websocket protocol is most likely blocked on the user's computer. In that case, provide a fallback connection over long polling.

It is worth switching to long polling completely only if the websocket connection has never come up. If it used to come up, switch temporarily and try websocket again — the built-in client returns to it after 30 minutes.

## Service Commands of the Server

Provide the handling of service commands in your client. Below is the content of the `text` field — the commands themselves arrive in the same envelope as application events.

### channel_expire

The server reports that the channel is about to expire.

```json
{
    "module_id": "pull",
    "command": "channel_expire",
    "params": {
        "action": "reconnect",
        "channel": {
            "id": "46a437d2336d4a88e4e9b3cd956ecf45.7910bb25e660bf211fdec15e33c5e25e4c3b644a",
            "type": "shared"
        },
        "new_channel": {
            "id": "fb9f7e13dc3d595c5aefe1a0216c27a2.2887eebc6ae160713a732893462dce9d8e23a7b0",
            "start": "2017-06-28T09:57:48+02:00",
            "end": "2017-06-28T21:57:48+02:00",
            "type": "shared"
        }
    }
}
```

#### Command Parameters

#|
|| **Name**
`type` | **Description** ||
|| **action**
[`string`](../../api-reference/data-types.md) | The action the client has to perform:

- `reconnect` — reconnect to the new channel from `new_channel`
- `get_config` — request the configuration again ||
|| **channel**
[`object`](../../api-reference/data-types.md) | Information about the channel the command was received for ||
|| **new_channel**
[`object`](../../api-reference/data-types.md) | Information about the new channel. It arrives only if `action` equals `reconnect` ||
|#

#### How to Handle the Command

When the `channel_expire` command arrives, perform the steps depending on the value of `action`:

- `action` equals `reconnect`
    - replace the information about the current channel with the data from `new_channel`
    - re-establish the connection to the server
- `action` equals `get_config`
    - disconnect from the server
    - request new channel data with the [pull.application.config.get](./pull-application-config-get.md) method
    - establish the connection to the server again

### config_expire and server_restart

The server reports that its settings have changed.

```json
{
    "module_id": "pull",
    "command": "config_expire",
    "params": {}
}
```

The `server_restart` command arrives in the same form, only the value of `command` differs:

```json
{
    "module_id": "pull",
    "command": "server_restart",
    "params": {}
}
```

#### How to Handle the config_expire and server_restart Commands

If the `config_expire` or the `server_restart` command has arrived:

- disconnect from the server
- after a random interval of 10 to 120 seconds, request new channel data with the [pull.application.config.get](./pull-application-config-get.md) method. The spread is needed so that the clients do not come for the configuration all at once after the server has restarted
- establish the connection to the server again

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./push-and-pull-in-browser.md)
- [{#T}](./pull-application-config-get.md)
- [{#T}](./pull-application-event-add.md)
- [{#T}](./pull-application-push-add.md)
