# Unregister Event Handler event.unbind

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `event.unbind` method deletes event handlers that the application registered with the [event.bind](./event-bind.md) method. BX24.js provides the [BX24.callUnbind](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-unbind.md) wrapper for this method.

The method works only within the authorization context of an [application](../../settings/app-installation/index.md). For a user without administrator rights, the method is available with the following restrictions:

- offline events are unavailable: a call with `event_type=offline` returns the `ACCESS_DENIED` error
- only the user's own online event handlers can be deleted

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../data-types.md) | Event code, for example `ONCRMLEADADD`. The code is case-insensitive ||
|| **handler***
[`string`](../data-types.md) | Handler URL specified at registration. Required for online events. With `event_type=offline`, the value is ignored ||
|| **auth_type**
[`integer`](../data-types.md) | User identifier under which the event handler is authorized. Without this parameter, an administrator deletes the handlers of all users, and a user without administrator rights deletes only their own. A user without administrator rights does not need to pass this parameter: if it is passed, only the user's own ID as an integer is allowed, while the string `"15"` or `0` returns the `ACCESS_DENIED` error. With `event_type=offline`, the parameter is ignored

{% note info %}

To delete only the handlers that are authorized on behalf of the user who triggered the event, an administrator passes `auth_type=0`. Handlers with a different `auth_type` remain.

{% endnote %}
||
|| **event_type**
[`string`](../data-types.md) | Subscription type: `online` or `offline`, case-insensitive. Defaults to `online`. With `offline`, the method works with [offline events](./offline-events.md) ||
|| **auth_connector**
[`string`](../data-types.md) | Source key. Applies only with `event_type=offline`: the method deletes offline handlers with the same `auth_connector` that was passed to [event.bind](./event-bind.md). Without this parameter, the method deletes handlers that have no source key ||
|#

The method deletes all handlers of the application that match the passed parameters.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "event": "ONCRMLEADADD",
        "handler": "https://www.my-domain.com/handler/",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/event.unbind
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EventUnbindResult = {
      count: number
    }

    try {
      const response = await $b24.actions.v2.call.make<EventUnbindResult>({
        method: 'event.unbind',
        params: {
          event: 'ONCRMLEADADD',
          handler: 'https://www.my-domain.com/handler/',
          auth_type: 15,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Unbound handlers count:', result.count)
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
      async function unbindEvent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'event.unbind',
            params: {
              event: 'ONCRMLEADADD',
              handler: 'https://www.my-domain.com/handler/',
              auth_type: 15,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Unbound handlers count:', result.count)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', unbindEvent)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.event.unbind(
            event="ONCRMLEADADD",
            handler="https://www.my-domain.com/handler/",
            auth_type=15,
        ).response
        result = bitrix_response.result
        print(result)
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
        $eventCode = 'your_event_code'; // Replace with your actual event code
        $handlerUrl = 'https://your.handler.url'; // Replace with your actual handler URL
        $userId = null; // Replace with your actual user ID or leave as null
        $result = $serviceBuilder
            ->getMainScope()
            ->event()
            ->unbind($eventCode, $handlerUrl, $userId);
        print($result->getUnbindedHandlersCount());
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'event.unbind',
        [
            'event' => 'ONCRMLEADADD',
            'handler' => 'https://www.my-domain.com/handler/',
            'auth_type' => 15
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The method returns the number of deleted handlers.

```json
{
    "result": {
        "count": 1
    },
    "time": {
        "start": 1721298360.468008,
        "finish": 1721298360.553977,
        "duration": 0.0859689712524414,
        "processing": 0.0023431777954101562,
        "date_start": "2024-07-18T12:26:00+02:00",
        "date_finish": "2024-07-18T12:26:00+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Deletion result [(detailed description)](#result) ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **count**
[`integer`](../data-types.md) | Number of deleted handlers. If no matching handlers are found, `0` ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied! Offline events unbinding requires administrator access rights"
}
```

{% include notitle [Error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_ARGUMENT` | Argument 'EVENT' is null or empty | The `event` parameter is not passed ||
|| `400` | `ERROR_ARGUMENT` | Argument 'HANDLER' is null or empty | The `handler` parameter is not passed for an online event ||
|| `400` | `ERROR_ARGUMENT` | ```Value must be one of {online|offline}``` | An invalid `event_type` value is passed ||
|| `403` | `ACCESS_DENIED` | Access denied! Offline events unbinding requires administrator access rights | The method was called by a non-administrator with `event_type=offline` ||
|| `403` | `ACCESS_DENIED` | Access denied! Event unbinding with AUTH_TYPE requires administrator access rights | The method was called by a non-administrator who passed an `auth_type` that is not their own ID as an integer ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | The method was called outside an application, for example, through a webhook ||
|| `403` | `WRONG_LICENSE` | This feature is not enabled for the current license: auth_connector | `auth_connector` is passed, but the plan does not support source keys ||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./events.md)
- [{#T}](./event-bind.md)
- [{#T}](./event-get.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
