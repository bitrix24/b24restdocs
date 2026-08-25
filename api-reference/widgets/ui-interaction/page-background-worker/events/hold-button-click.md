# When Clicking the Hold Call Button BackgroundCallCard::holdButtonClick

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can subscribe: any user

The `BackgroundCallCard::holdButtonClick` event occurs when the operator clicks the hold button.

{% note info "" %}

The event operates within the application context in the `PAGE_BACKGROUND_WORKER` placement. This is a JS interface event, not a REST event: you cannot subscribe to it with a request to `/rest/`.

{% endnote %}

## What the Handler Receives

Data is passed to the callback `BX24.placement.bindEvent` {.b24-info}

```js
callback(true);
```

## Event Handler Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **eventData***
[`boolean`](../../../../data-types.md) | The current hold state after the button is clicked.

Possible values:

- `true` — on hold
- `false` — taken off hold ||
|#

## Event Subscription Parameters

The handler is registered from the widget with the [BX24.placement.bindEvent](../../bx24-placement-bind-event.md) method.

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../../../data-types.md) | The name of the interface event.

For this event — `BackgroundCallCard::holdButtonClick` ||
|| **callback***
[`callable`](../../../../data-types.md) | The function Bitrix24 invokes when the event occurs. The handler arguments are described above ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.bindEvent('BackgroundCallCard::holdButtonClick', function (eventData) {
                console.log(eventData);
            });
        });
    });
    ```

- JS (TS)

    ```ts
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide)
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    await $b24.placement.bindEvent('BackgroundCallCard::holdButtonClick', (eventData: boolean) => {
      console.log(eventData)
    })
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        await $b24.placement.bindEvent('BackgroundCallCard::holdButtonClick', (eventData) => {
          console.log(eventData)
        })
      })
    </script>
    ```


- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.placement.bind_event(
            placement='BackgroundCallCard::holdButtonClick',
            handler='**your_handler_url_here**',
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
{% endlist %}

## Errors

Check the following conditions.

- The widget is open in the `PAGE_BACKGROUND_WORKER` placement. In other placements, the `BackgroundCallCard::*` events are not registered, and the subscription silently fails
- The event name is passed without typos and with the correct capitalization. The list of events available in the current placement is returned by [BX24.placement.getInterface](../../bx24-placement-get-interface.md)
- The call was raised by the application with the [telephony.externalCall.register](../../../../telephony/telephony-external-call-register.md) method. For calls made by Bitrix24 itself, the `BackgroundCallCard::*` events are not emitted at all

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../card.md)
- [{#T}](../index.md)
