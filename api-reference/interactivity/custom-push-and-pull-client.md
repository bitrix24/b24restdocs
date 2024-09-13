# Custom Push&Pull Client

In this article, you will find information on how to write your own client. If you don't need a new client but just want to add interactivity to an existing application, you may find the article [{#T}](./push-and-pull-in-browser.md) useful.

By connecting to RT servers, you can create a truly interactive application: the application's state changes, and the interface updates instantly without the need for AJAX requests.

## Application Development

There are two ways to connect to the service: Long polling and WebSocket.

{% note tip " " %}

When a user accesses the site, their browser, desktop, or mobile application establishes and maintains a persistent connection with the Push server. This is usually done using **WebSocket**, which is supported by 95% of modern browsers.

If the WebSocket technology is not supported by the browser, **Long Polling** is used instead — a persistent long polling method.

{% endnote %}

It is recommended to use WebSocket as the primary connection method. Long polling is only used for devices that do not support WebSocket or if the client frequently encounters issues when connecting to WebSocket.

To obtain data about the server and connection addresses, use the REST command [pull.application.config.get](./push-and-pull/pull-application-config-get.md).

To connect to the server, take the connection address of the desired type (for example, `websocket_secure`) from the server settings (the `server` field) and append all available channels through `/`. If a cloud push server is used on the account, you need to add the `clientId` parameter to the URL for connecting to the server.

Example connection for WebSocket for a cloud push server:

```bash
wss://rtc-cloud-ms1.bitrix.info/subws/?CHANNEL_ID=beb502091dfc9b93d7fd648aa4ec332e%3A7cc478c89de71ec78bf4820d3d814a3e.4f5466742ca1e59e263fee732a7dbe002889ba91%2F1ab4f7a440cea35a1abccd5c2566c688.b33914ef342e5cd21e4fbcf4ac92acd2e9ea3755&clientId=fcda45d0859442735f07b8bb5825ded1
```

Example connection for WebSocket for an on-premise push server:

```bash
wss://rt.bitrix24.com/sub/?CHANNEL_ID=46a437d2336d4a88e4e9b3cd956ecf45:6221e0eb48981fce67cf4756e82e8102.7910bb25e660bf211fdec15e33c5e25e4c3b644a/fb9f7e13dc3d595c5aefe1a0216c27a2.2887eebc6ae160713a732893462dce9d8e23a7b0
```

The lifetime of each channel is limited to 12 hours. You need to monitor the expiration time and make a repeated request to [pull.application.config.get](./push-and-pull/pull-application-config-get.md) when one of the channels has expired.

To access the history in the channel, append special GET parameters to the above address: `&tag=` and `&time=` (for server version 2 and below) or `&mid=` (for version 3 and above). You will receive all necessary data for the GET parameters when parsing each command from the channel. After connecting with these parameters, you will have one-time access to messages that were not available for the current session.

## General Format of Commands from the Server

For server version 3 and below, when commands are received, the text appears as follows:

```bash
#!NGINXNMS!#{"id":320146,"mid":"14526134350000000000320146","channel":"6221e0eb48981fce67cf4756e82e8102","tag":"672","time":"Thu, 29 Jun 2017 09:50:16 GMT","text":{...},"extra":{...}}}#!NGINXNME!#
#!NGINXNMS!#{"id":320147,"mid":"14526134350000000000320147","channel":"6221e0eb48981fce67cf4756e82e8102","tag":"673","time":"Thu, 29 Jun 2017 09:50:17 GMT","text":{...},"extra":{...}}}#!NGINXNME!#
```

To work with the command, you need to extract its content and convert it to JSON. The command text is located between the control phrases #!NGINXNMS!# and #!NGINXNME!#.

Starting from server version 4, use the addition of the GET parameter `&format=json` when connecting and when commands are received; it will be returned to you in JSON format:

```json
[
    {"id":320146,"mid":"14526134350000000000320146","channel":"6221e0eb48981fce67cf4756e82e8102","tag":"672","time":"Thu, 29 Jun 2017 09:50:16 GMT","text":{...},"extra":{...}}},
    {"id":320147,"mid":"14526134350000000000320147","channel":"6221e0eb48981fce67cf4756e82e8102","tag":"673","time":"Thu, 29 Jun 2017 09:50:17 GMT","text":{...},"extra":{...}}}
]
```

The JSON structure of the command has the following unified format:

```json
{
    "id" : 320146,
    "mid" : "14526134350000000000320146",
    "channel" : "6221e0eb48981fce67cf4756e82e8102",
    "tag" : "672",
    "time" :"Mon, 03 Oct 2017 06:36:01 GMT",
    "text" : {
        "module_id" : "main",
        "command" : "user_online",
        "params" : {
            ...
        },
    },
    "extra" : {
        "server_time": "2017-10-03T08:36:01+02:00",
        "server_time_unix": 1507012561,
        "server_time_ago": 0,
        "revision": 16,
        "revisionMobile": 1,
        "channel" : "6221e0eb48981fce67cf4756e82e8102"
    }
}
```

Where:

- `id` — message identifier
- `mid` — message identifier (only for server version 3 and above, used for restoring history from a specific message)
- `channel` — channel identifier (only for server version 3 and above, for earlier versions use `extra.channel`)
- `tag` — E-tag (for server version 2 and below, used for restoring history from a specific message)
- `time` — message time (for server version 2 and below, used for restoring history from a specific message)
- `text` — structure describing the command's action, containing the following keys:
    - `module_id` — identifier of the module that sent the command (for marketplace applications — `appId`)
    - `command` — command identifier
    - `params` — additional data for executing the command
- `extra` — structure describing additional information:
    - `server_time` — server time at the moment the command was generated (ATOM format)
    - `server_time_unix` — server time at the moment the command was generated (in Unix timestamp format with the browser's timezone)
    - `server_time_ago` — number of seconds that have passed since the command was sent
    - `server_name` — name of the server that sent the command
    - `revision` — revision of the push & pull module for the browser script
    - `revisionMobile` — revision of the push & pull module for the mobile client
    - `channel` — channel identifier (only for server version 1)

## Error Handling

During operation with the server, various errors may occur. To avoid being blocked for suspicious activity, it is important to handle them correctly.

If connecting to the server results in errors, you should incrementally increase the connection time:

- When an error occurs for the first time, the connection delay is 100ms.
- On a repeated error - 15 seconds.
- From 3 to 5 errors - 45 seconds.
- From 5 to 10 errors - 10 minutes.
- More than 10 consecutive errors - 1 hour.

When working with WebSocket, if errors occur more than 4 times, it is recommended to switch to Long polling. It is very likely that the WebSocket protocol is blocked on the client's computer (WebSocket closure codes `1006` and `1008`).

A full switch to Long polling makes sense if you have never been able to connect via WebSocket. If there have been successful connections before, it makes sense to switch temporarily for 10 to 30 minutes.

## Format of Incoming Commands for Connection Management

For your own implementation of the Push & Pull protocol, ensure you handle control commands.

### channel_expire

Command about the expiration of the channel's lifetime.

```json
{
    "module_id" : "pull",
    "command" : "channel_expire",
    "params" : {
        "action" : "reconnect",
        "channel" : {
            "id" : "46a437d2336d4a88e4e9b3cd956ecf45.7910bb25e660bf211fdec15e33c5e25e4c3b644a",
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
|| **Name** | **Description** ||
|| **action** | Description of the required action ||
|| **channel** | Information about the channel for which the command was received ||
|| **new_channel** | Information about the new channel. Available only if `action == reconnect` ||
|#

#### Description of Command Handling

Upon receiving the `channel_expire` command, depending on the value of `action`, perform the following steps:

- `action == reconnect`
  - replace the current channel information with data from `new_channel`
  - re-establish the connection to the server
- `action == get_config`
  - disconnect from the server
  - request new channel data via REST [pull.application.config.get](./push-and-pull/pull-application-config-get.md)
  - re-establish the connection to the server

### config_expire and server_restart

Command about changes to the server settings.

```json
{
	"module_id" : "pull",
	"command" : "config_expire",
	"params" : {}
}
```

#### Description of Command Handling

If the `config_expire` or `server_restart` command is received:
- disconnect from the server
- after a random interval (from 10 to 120 seconds), request new channel data via REST [pull.application.config.get](./push-and-pull/pull-application-config-get.md)
- re-establish the connection to the server