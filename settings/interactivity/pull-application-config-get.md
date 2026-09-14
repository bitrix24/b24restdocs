# Get the Connection Configuration for Push&Pull Servers pull.application.config.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`pull`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: any user authorized in the application

The method `pull.application.config.get` returns the connection configuration for the Push&Pull servers for the current application.

{% note info "" %}

The method works only in the context of an [application](../app-installation/index.md). The request is executed with the application OAuth token and the `pull` scope, and a webhook does not create such a context.

{% endnote %}

The client builds the connection address from the method response. The server address is taken from `server.websocket_secure` or `server.long_pooling_secure`, and the channel identifiers are taken from `channels.private.id` and `channels.shared.id` and are substituted into the address in exactly that order. How to build the address manually is described in the [{#T}](./custom-push-and-pull-client.md) article; in the browser this is done by the built-in client — [{#T}](./push-and-pull-in-browser.md).

The configuration is personal: `channels.private` and `publicChannels` belong to the user whose token the request is executed with. One configuration cannot be handed out to several users — request it for each of them separately.

Track the `end` time of each channel and the `exp` field in the response. When the time expires, request the configuration again. If there is no `exp` field in the response, rely on the `end` of the channel.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

The method has no required parameters.

#|
|| **Name**
`type` | **Description** ||
|| **CACHE**
[`string`](../../api-reference/data-types.md) | The indicator of cache usage:

- `N` — do not use the cache
- any other value — use the cache

By default, the cache is used.

In the current version of Bitrix24 the parameter has no effect on the returned configuration: the cache is always used ||
|| **REOPEN**
[`string`](../../api-reference/data-types.md) | The indicator of issuing a new channel if the current one has expired:

- `N` — do not issue
- any other value — issue

By default, a new channel is issued.

In the current version of Bitrix24 the parameter has no effect on the returned configuration: an expired channel is always issued anew ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

An example of retrieving the Push&Pull configuration for an application. The method is called without parameters.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/pull.application.config.get.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<Record<string, any>>({
        method: 'pull.application.config.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Push&Pull config:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getApplicationPullConfig() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'pull.application.config.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Push&Pull config:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getApplicationPullConfig)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.pull.application.config.get().response
        print(bitrix_response.result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call('pull.application.config.get');

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting pull config: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'pull.application.config.get',
        {},
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call('pull.application.config.get');

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "server": {
            "version": 4,
            "server_enabled": true,
            "mode": "personal",
            "hostname": "your-account.bitrix24.com",
            "long_polling": "https://rtc-**.bitrix24.com/sub2/",
            "long_pooling_secure": "https://rtc-**.bitrix24.com/sub2/",
            "websocket_enabled": true,
            "websocket": "wss://rtc-**.bitrix24.com/subws2/",
            "websocket_secure": "wss://rtc-**.bitrix24.com/subws2/",
            "publish_enabled": true,
            "publish": "https://rtc-**.bitrix24.com/rest/",
            "publish_secure": "https://rtc-**.bitrix24.com/rest/",
            "config_timestamp": 1774886062
        },
        "api": {
            "revision_web": 19,
            "revision_mobile": 3
        },
        "channels": {
            "shared": {
                "id": "***masked***",
                "start": "2026-03-31T17:05:18+02:00",
                "end": "2026-04-01T05:05:23+02:00",
                "type": "shared"
            },
            "private": {
                "id": "***masked***",
                "public_id": "***masked***",
                "start": "2026-03-31T17:05:18+02:00",
                "end": "2026-04-01T05:05:23+02:00",
                "type": "private"
            }
        },
        "exp": 1775052318,
        "publicChannels": {
            "577": {
                "user_id": 577,
                "public_id": "***masked***",
                "signature": "***masked***",
                "start": "2026-03-31T10:06:39+02:00",
                "end": "2026-03-31T22:06:44+02:00"
            }
        }
    },
    "time": {
        "start": 1774965918,
        "finish": 1774965918.322255,
        "duration": 0.32225489616394043,
        "processing": 0,
        "date_start": "2026-03-31T17:05:18+02:00",
        "date_finish": "2026-03-31T17:05:18+02:00",
        "operating_reset_at": 1774966518,
        "operating": 0
    }
}
```

If Bitrix24 runs on the shared Push&Pull server, `clientId` additionally arrives in `result`. The example shows only the differences from the response above:

```json
{
    "result": {
        "server": {
            "mode": "shared",
            "version": 4
        },
        "clientId": "***masked***"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../api-reference/data-types.md) | An object of the format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — a field of the `result` object
- `value_n` — the value of the `result` field

The fields of the `result` object — [(detailed description)](#result) ||
|| **time**
[`time`](../../api-reference/data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

The composition of the fields depends on the settings of the Push&Pull server — the `exp`, `clientId`, and `jwt` fields do not always arrive.

#|
|| **Name**
`type` | **Description** ||
|| **server**
[`object`](../../api-reference/data-types.md) | The parameters of the Push&Pull server [more details](#result-server-type) ||
|| **api**
[`object`](../../api-reference/data-types.md) | The revisions of the Push&Pull protocol. The client uses them to check whether its library version is compatible with the server:

- **revision_web** [`integer`](../../api-reference/data-types.md) — the revision for the browser client
- **revision_mobile** [`integer`](../../api-reference/data-types.md) — the revision for the mobile client ||
|| **channels**
[`object`](../../api-reference/data-types.md) | The application channels.

It contains two channels: `shared` — the common channel of the application, and `private` — the personal channel of the user on whose behalf the request is executed.

The channel fields — [(detailed description)](#result-channel-type). The order in which the channel identifiers are substituted into the connection address is described in the [{#T}](./custom-push-and-pull-client.md) article ||
|| **exp**
[`integer`](../../api-reference/data-types.md) | The validity period of the configuration as a Unix timestamp. In Bitrix24 cloud the configuration is valid for 24 hours from the moment of the request.

In the self-hosted version the field arrives if the administrator has set this period. With server version 5 and higher the field always arrives and matches the validity period of `jwt` ||
|| **publicChannels**
[`object`](../../api-reference/data-types.md) \| [`boolean`](../../api-reference/data-types.md) | The public identifier of the personal channel of the current user. This is the user's channel in Bitrix24, not a channel of the application.

If the version of the Push&Pull server is lower than 4, the field arrives with the `false` value.

Format:

```
{
    "<user_id>": {
        "user_id": user_id,
        "public_id": "string",
        "signature": "string",
        "start": "datetime",
        "end": "datetime"
    }
}
```

where:
- `<user_id>` — the key of the object, the user identifier
- **user_id** [`integer`](../../api-reference/data-types.md) — the user identifier inside the channel object
- **public_id** [`string`](../../api-reference/data-types.md) — the public identifier of the channel
- **signature** [`string`](../../api-reference/data-types.md) — the channel signature
- **start** [`datetime`](../../api-reference/data-types.md) — the time the channel was issued
- **end** [`datetime`](../../api-reference/data-types.md) — the time the channel stops working ||
|| **clientId**
[`string`](../../api-reference/data-types.md) | The public identifier of Bitrix24 on the shared Push&Pull server.

It is returned when `server.mode` equals `shared`. In that case, add `clientId` to the server connection address — [{#T}](./custom-push-and-pull-client.md) ||
|| **jwt**
[`string`](../../api-reference/data-types.md) | The JWT token for connecting to the server. When it is present, the channels are already embedded in the token and `CHANNEL_ID` is not passed in the connection address — [{#T}](./custom-push-and-pull-client.md).

It is returned when `server.version` equals 5 or higher. On the shared Push&Pull server the token is not issued, so `jwt` and `clientId` never occur in the same response ||
|#

##### Server Object {#result-server-type}

#|
|| **Name**
`type` | **Description** ||
|| **version**
[`integer`](../../api-reference/data-types.md) | The version of the Push&Pull server. The connection protocol and the way the channel history is handled depend on it — the details are in the [Custom Push&Pull Client](./custom-push-and-pull-client.md) article.

In the `shared` mode the version is always 4. In the `personal` mode the value is set by the administrator, 2 by default ||
|| **server_enabled**
[`boolean`](../../api-reference/data-types.md) | The indicator of server availability. If the value is `false`, there is nothing to connect to — Push&Pull is not configured in this Bitrix24 ||
|| **mode**
[`string`](../../api-reference/data-types.md) | The server mode:

- `personal` — a self-hosted Push&Pull server
- `shared` — the shared Push&Pull server ||
|| **hostname**
[`string`](../../api-reference/data-types.md) | The Bitrix24 domain ||
|| **long_polling**
[`string`](../../api-reference/data-types.md) | The long polling URL ||
|| **long_pooling_secure**
[`string`](../../api-reference/data-types.md) | The long polling URL for a secure connection.

The field name arrives from the API with a typo — `pooling` instead of `polling`. Read it as is, otherwise parsing the response will break ||
|| **websocket_enabled**
[`boolean`](../../api-reference/data-types.md) | The indicator of websocket availability. If the value is `false`, connect over long polling ||
|| **websocket**
[`string`](../../api-reference/data-types.md) | The websocket URL ||
|| **websocket_secure**
[`string`](../../api-reference/data-types.md) | The websocket URL for a secure connection.

The connection parameters are passed in the address, so connect over the secure address. On the shared Push&Pull server `websocket` and `websocket_secure` match; on a self-hosted one they are set by the administrator ||
|| **publish_enabled**
[`boolean`](../../api-reference/data-types.md) | The indicator of publish API availability ||
|| **publish**
[`string`](../../api-reference/data-types.md) | The publish API URL ||
|| **publish_secure**
[`string`](../../api-reference/data-types.md) | The publish API URL for a secure connection ||
|| **config_timestamp**
[`integer`](../../api-reference/data-types.md) | The version stamp of the configuration ||
|#

##### Shared and Private Channel Object {#result-channel-type}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../api-reference/data-types.md) | The channel identifier ||
|| **public_id**
[`string`](../../api-reference/data-types.md) | The public identifier of the channel.

For the `shared` channel the field is not returned. For the `private` channel the field arrives as an empty string if the version of the Push&Pull server is 3 or lower ||
|| **start**
[`datetime`](../../api-reference/data-types.md) | The time the channel was issued ||
|| **end**
[`datetime`](../../api-reference/data-types.md) | The time the channel stops working — 12 hours after it was issued ||
|| **type**
[`string`](../../api-reference/data-types.md) | The channel type:

- `shared`
- `private` ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Get access to application config available only for application authorization."
}
```

HTTP Status: **500**

```json
{
    "error": "SERVER_ERROR",
    "error_description": "Push & Pull server is not configured"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Get access to application config available only for application authorization. | The method was called outside the context of an application, for example, through a webhook ||
|| `500` | `SERVER_ERROR` | Push & Pull server is not configured | The Push&Pull server is not configured or is disabled in Bitrix24 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./push-and-pull-in-browser.md)
- [{#T}](./custom-push-and-pull-client.md)
- [{#T}](./pull-application-event-add.md)
- [{#T}](./pull-application-push-add.md)
