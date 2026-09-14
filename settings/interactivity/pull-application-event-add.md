# Send an Event to the Application Channel pull.application.event.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`pull`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: any user authorized in the application. Only a Bitrix24 administrator can send an event to another user's channel

The method `pull.application.event.add` sends an event to the application channel. The event is received by the [built-in client in the browser](./push-and-pull-in-browser.md) or by a [custom client](./custom-push-and-pull-client.md) connected to the channel. This is how the interface of an open application is updated; to notify the user outside the Bitrix24 interface, a different method is needed — [pull.application.push.add](./pull-application-push-add.md).

An event is retained in the Push&Pull queue for 24 hours, so it can be taken from the history after connecting late — how to do that is described in the [{#T}](./custom-push-and-pull-client.md) article. Do not confuse this period with the 12 hours: that is how long the channel identifier lives, and after it is reissued the history is read on by `mid` or by the `tag` and `time` pair.

The channel identifiers and the server addresses are retrieved by the client beforehand with the [pull.application.config.get](./pull-application-config-get.md) method. The `MODULE_ID` and `COMMAND` values from the request have to match what the client has subscribed to, otherwise the handler will not receive the event.

{% note info "" %}

The method works only in the context of an [application](../app-installation/index.md). The request is executed with the application OAuth token and the `pull` scope, and a webhook does not create such a context.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **COMMAND**^*^
[`string`](../../api-reference/data-types.md) | The event command.

Allowed characters: `A-Z`, `a-z`, `0-9`, `_`, `:`, ```|```, `.`, `-`

The client uses the command to distinguish event types and decide what to update in the interface ||
|| **PARAMS**
[`object`](../../api-reference/data-types.md) | The event parameters in the format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — the name of the event parameter
- `value_n` — the value of the event parameter

You set the field names and values yourself. If the parameter is not passed, the client receives the command with an empty `params`.

The size of the message that Bitrix24 sends to the Push&Pull server is limited to 1 MB. In the self-hosted version the administrator can change the limit with the `limit_max_payload` setting of the `pull` module.

Example:

```json
{
    "grid_id": 15,
    "status": "done"
}
``` ||
|| **MODULE_ID**
[`string`](../../api-reference/data-types.md) | The identifier of the event module.

Allowed characters: `a-z`, `0-9`, `.`, `_`

`application` is used by default ||
|| **USER_ID**
[`integer`](../../api-reference/data-types.md) \| [`string`](../../api-reference/data-types.md) \| [`integer[]`](../../api-reference/data-types.md) | A user identifier or an array of user identifiers.

`USER_ID` can be retrieved:
- with the [user.get](../../api-reference/user/user-get.md) method
- with the [user.current](../../api-reference/user/user-current.md) method for the current user

A Bitrix24 administrator can specify any users and pass an array of identifiers.

A user without administrator permissions can specify only their own identifier, and only as a string — `"USER_ID": "577"`. Bitrix24 compares the value with the identifier of the current user strictly by type, so the method rejects both the number `577` and the array `["577"]` with the `USER_ID_ACCESS_ERROR` error.

If the parameter is not passed, the event is sent to the common `shared` channel, and with the parameter it is sent to the `private` personal channel of the specified user. The client has to be subscribed to the required channel — [Subscribing to Events](./push-and-pull-in-browser.md#subscribe) ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

An example of sending an event to the common channel of an application, where:
- `COMMAND` — the event command
- `PARAMS` — the event parameters
- `MODULE_ID` — the identifier of the event module

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "COMMAND": "test_event",
        "PARAMS": {
          "param1": "value1"
        },
        "MODULE_ID": "application",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/pull.application.event.add.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'pull.application.event.add',
        params: {
          COMMAND: 'test_event',
          PARAMS: {
            param1: 'value1',
          },
          MODULE_ID: 'application',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Event accepted:', result)
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
      async function sendApplicationEvent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'pull.application.event.add',
            params: {
              COMMAND: 'test_event',
              PARAMS: {
                param1: 'value1',
              },
              MODULE_ID: 'application',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Event accepted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', sendApplicationEvent)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # The method is called directly: in the typed wrapper client.pull.application.event.add
    # the params argument accepts a list rather than an object, so PARAMS cannot be passed through it
    try:
        bitrix_response = client.call(
            "pull.application.event.add",
            {
                "COMMAND": "test_event",
                "PARAMS": {
                    "param1": "value1",
                },
                "MODULE_ID": "application",
            },
        ).response
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
            ->call(
                'pull.application.event.add',
                [
                    'COMMAND' => 'test_event',
                    'PARAMS' => [
                        'param1' => 'value1',
                    ],
                    'MODULE_ID' => 'application',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sending event: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'pull.application.event.add',
        {
            COMMAND: 'test_event',
            PARAMS: {
                param1: 'value1'
            },
            MODULE_ID: 'application'
        },
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
    $result = CRest::call(
        'pull.application.event.add',
        [
            'COMMAND' => 'test_event',
            'PARAMS' => [
                'param1' => 'value1',
            ],
            'MODULE_ID' => 'application',
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

The examples send the event to the common channel. To send it to a personal channel, add `USER_ID` to the request:

```json
{
    "COMMAND": "test_event",
    "PARAMS": {
        "param1": "value1"
    },
    "MODULE_ID": "application",
    "USER_ID": "577"
}
```

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1743495945,
        "finish": 1743495945.285066,
        "duration": 0.2850658893585205,
        "processing": 0.008597135543823242,
        "date_start": "2025-04-01T11:52:25+02:00",
        "date_finish": "2025-04-01T11:52:25+02:00",
        "operating_reset_at": 1743496545,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../api-reference/data-types.md) | The indicator that the event has been accepted for sending. The method returns `true` even if the event did not reach the channel — for example, when the Push&Pull server did not accept the event because of its size ||
|| **time**
[`time`](../../api-reference/data-types.md#time) | Information about the request execution time. The composition of the fields — [Time Object](../../api-reference/data-types.md#time) ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Get access to application config available only for application authorization."
}
```

HTTP Status: **400**

```json
{
    "error": "COMMAND_ERROR",
    "error_description": "Command format error"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Get access to application config available only for application authorization. | The method was called outside the context of an application, for example, through a webhook. The error text is the same as the text of the same error of the [pull.application.config.get](./pull-application-config-get.md) method — tell them apart by the name of the method called ||
|| `400` | `USER_ID_ACCESS_ERROR` | Only admin can send notifications to other channels | A user without administrator permissions is trying to send an event to someone else's channel, or is passing their own `USER_ID` as a number or an array instead of a string ||
|| `400` | `MODULE_ID_ERROR` | Module ID format error | The `MODULE_ID` parameter contains disallowed characters ||
|| `400` | `COMMAND_ERROR` | Command format error | The `COMMAND` parameter is not passed, is empty, or contains disallowed characters ||
|| `400` | `PARAMS_ERROR` | Params format error | The `PARAMS` parameter is not passed as an object ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./push-and-pull-in-browser.md)
- [{#T}](./custom-push-and-pull-client.md)
- [{#T}](./pull-application-config-get.md)
- [{#T}](./pull-application-push-add.md)
