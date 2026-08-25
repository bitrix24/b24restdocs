# Before Close Card Event CallCard::BeforeClose

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — access to the call card placement
>
> Who can subscribe: any user

The `CallCard::BeforeClose` event occurs when Bitrix24 tries to close the call card.

The card may not close. If auto-closing is blocked by the [disableAutoClose](./disable-auto-close.md) command or a comment form is open in the card, the event still arrives while the card stays on screen for another 65 seconds. When the card actually closes, the event arrives a second time, so the handler must be ready to run twice.

{% note info "" %}

The event operates within the application context in the `CALL_CARD` placement. This is a JS interface event, not a REST event: you cannot subscribe to it with a request to `/rest/`.

{% endnote %}

## What the Handler Receives

No data is passed to the event handler.

## Event Subscription Parameters

The handler is registered from the widget with the [BX24.placement.bindEvent](../bx24-placement-bind-event.md) method.

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | The name of the interface event.

For this event — `CallCard::BeforeClose` ||
|| **callback***
[`callable`](../../../data-types.md) | The function Bitrix24 invokes when the event occurs. No arguments are passed to the handler ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.bindEvent('CallCard::BeforeClose', function () {
                // handler code
            });
        });
    });
    ```

- JS (TS)

    ```ts
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide)
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    await $b24.placement.bindEvent('CallCard::BeforeClose', () => {
      // handler code
    })
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        await $b24.placement.bindEvent('CallCard::BeforeClose', () => {
          // handler code
        })
      })
    </script>
    ```

{% endlist %}

## Errors

Check the following conditions.

- The widget is open in the `CALL_CARD` placement. In other placements, the `CallCard::*` events are not registered, and the subscription silently fails
- The event name is passed without typos and with the correct capitalization. The list of events available in the current placement is returned by [BX24.placement.getInterface](../bx24-placement-get-interface.md)

## Continue Learning

- [{#T}](./get-status.md)
- [{#T}](./disable-auto-close.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-entity-changed.md)
- [{#T}](./call-card-call-state-changed.md)
- [{#T}](./index.md)
- [{#T}](../../telephony/call-card.md)
