# Get a List of Registered Event Handlers event.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Who can execute the method: any user

The `event.get` method allows you to retrieve a list of registered event handlers.

The method works only in the context of authorizing the [application](../../settings/app-installation/index.md).

No parameters.

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/event.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each event handler returned in result[]
    type EventHandlerItem = {
      event: string,
      handler: string,
      auth_type: string,
      offline: number,
    }

    try {
      const response = await $b24.actions.v2.call.make<EventHandlerItem[]>({
        method: 'event.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(
          'Registered event handlers:',
          result.map(h => `${h.event} → ${h.handler}`)
        )
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
      async function getEventHandlers() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'event.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(
            'Registered event handlers:',
            result.map(h => `${h.event} → ${h.handler}`)
          )
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEventHandlers)
    </script>
    ```

- PHP

    ```php        
    try {
        $eventService = $serviceBuilder->getMainScope()->event();
        $result = $eventService->get();
        $eventHandlers = $result->getEventHandlers();
        foreach ($eventHandlers as $handler) {
            print("Event: " . $handler->event . "\n");
            print("Handler: " . $handler->handler . "\n");
            print("Auth Type: " . $handler->auth_type . "\n");
            print("Offline: " . $handler->offline . "\n");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "event.get",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'event.get',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}


## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "event": "ONCRMLEADADD",
            "handler": "https:\/\/www.my-domain.com\/handler\/",
            "auth_type": "0",
            "offline": 0
        },
        {
            "event": "ONCRMLEADADD",
            "handler": "https:\/\/www.my-domain.com\/handler\/",
            "auth_type": "15",
            "offline": 0
        }
    ],
    "time": {
        "start": 1721297941.536696,
        "finish": 1721297941.661148,
        "duration": 0.12445211410522461,
        "processing": 0.0029609203338623047,
        "date_start": "2024-07-18T12:19:01+02:00",
        "date_finish": "2024-07-18T12:19:01+02:00",
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

## Continue Learning

- [{#T}](./events.md)
- [{#T}](./event-bind.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)