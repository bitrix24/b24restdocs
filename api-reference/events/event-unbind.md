# Unbind Registered Event Handler event.unbind

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Who can execute the method: administrator

The `event.unbind` method cancels a registered event handler.

This method works only in the context of [application](../../settings/app-installation/index.md) authorization and only when authorized under a user with administrative rights to the account.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../data-types.md) | Event name ||
|| **handler***
[`string`](../data-types.md) | Link to the event handler ||
|| **auth_type**
[`integer`](../data-types.md) | User identifier under which the event handler is authorized.

{% note info %}

If you need to remove event handlers set with an empty `auth_type` (authorized on behalf of the user who triggered the event), but keep the other handlers, specify `auth_type=0` or an empty value for the parameter.

{% endnote %} 
||
|| **event_type**
[`string`](../data-types.md) | Values: `online\|offline`. By default, `event_type=online`, and the method's behavior remains unchanged. If `event_type=offline` is called, the method works with [offline events](./offline-events.md) ||
|#

If any parameters are not specified, all event handlers that meet the other requirements will be removed.

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
          console.info('Unbound handlers count:', result.count)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', unbindEvent)
    </script>
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'event.unbind',
        [
            'EVENT' => 'ONCRMLEADADD',
            'HANDLER' => 'https://www.my-domain.com/handler/',
            'AUTH_TYPE' => 15
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)

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

{% endlist %}

## Response Handling

HTTP status: **200**

The method returns the number of event handlers removed upon invocation.

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
[`object`](../data-types.md) | Root element of the response ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## See Also

- [{#T}](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-unbind.md)

## Continue Learning

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