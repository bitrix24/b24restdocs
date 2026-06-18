# Register a New Event Handler event.bind

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Who can execute the method: any user

The `event.bind` method registers a new event handler.

This method works only in the context of authorizing the [application](../../settings/app-installation/index.md). It can operate under a user with portal administration rights as well as under a regular user. The method for a user without administrator rights is available with limitations:

1. Offline events are not available; attempting to set them will generate an exception.
2. Events are set on behalf of the current user (see the description of the `auth_type` parameter). Explicitly specifying an `auth_type` different from the current user's `ID` will also generate an exception.

{% note info %}

Since requests will come from Bitrix servers, any URL must be accessible for GET/POST requests from the outside.

{% endnote %}

The interface for this method is [BX24.callBind](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-bind.md).

{% note info %}

When deleting and updating the application, its actions will be removed. Therefore, in the installer of each version, they need to be set from scratch.

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **event*** 
[`string`](../data-types.md) | Event name ||
|| **handler*** 
[`string`](../data-types.md) | Link to the event handler ||
|| **auth_type** 
[`integer`](../data-types.md) | Identifier of the user under whom the event handler is authorized. By default, the authorization of the user whose actions triggered the event will be used ||
|| **event_type** 
[`string`](../data-types.md) | Values: `online|offline`. By default, `event_type=online`, and the method's behavior remains unchanged. If `event_type=offline` is called, the method works with [offline events](./offline-events.md) ||
|| **auth_connector** 
[`string`](../data-types.md) | Source key. This parameter is intended for [offline events](./offline-events.md). It allows excluding false event triggers ||
|| **options** 
[`object`](../data-types.md) | Additional settings for the registered event. The set of fields depends on the event.

For the `ONOFFLINEEVENT` event, the `minTimeout` parameter is supported — the minimum interval between notifications in seconds. Default is 1. More details in the article [{#T}](./on-offline-event.md#min-timeout) ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

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
    https://**put_your_bitrix24_address**/rest/event.bind
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
        method: 'event.bind',
        params: {
          event: 'ONCRMLEADADD',
          handler: 'https://www.my-domain.ru/handler/',
          auth_type: 15,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Handler registered:', result)
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
      async function bindEvent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'event.bind',
            params: {
              event: 'ONCRMLEADADD',
              handler: 'https://www.my-domain.ru/handler/',
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
          console.info('Handler registered:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindEvent)
    </script>
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'event.bind',
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

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1721296536.908506,
        "finish": 1721296537.007365,
        "duration": 0.09885907173156738,
        "processing": 0.03251290321350098,
        "date_start": "2024-07-18T11:55:36+02:00",
        "date_finish": "2024-07-18T11:55:37+02:00",
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name** 
`type` | **Description** ||
|| **result** 
[`boolean`](../data-types.md) | Success of execution ||
|| **time** 
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_EVENT_NOT_FOUND",
    "error_description":"Event not found"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Error Message** | **Description** ||
|| `ERROR_EVENT_NOT_FOUND` | Event not found | The event is incorrectly specified ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./events.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
- [{#T}](../../tutorials/openlines/example-connector.md)