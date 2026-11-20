# When Clicking the "Hang Up" Button BackgroundCallCard::hangupButtonClick

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`](../../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can subscribe: any user

The `BackgroundCallCard::hangupButtonClick` event occurs when the operator clicks the "Hang Up" button in the call card.

The button is available in the `connectingIncoming`, `connectingOutgoing`, `connected`, and `transferConnected` states. The application enables these states with the [CallCardSetUiState](../call-card-set-ui-state.md) command. The event only reports the click: Bitrix24 does not end the call or close the card. The application ends the conversation in its own WebRTC client, records the end of the call with the [telephony.externalCall.finish](../../../../telephony/telephony-external-call-finish.md) method, and closes the card with the [CallCardClose](../call-card-close.md) command. The event does not pass `CALL_ID`, so save it in advance — it is returned in the `telephony.externalCall.register` response and in the [BackgroundCallCard::initialized](./initialized.md) event.

{% note info "" %}

The event operates within the application context in the `PAGE_BACKGROUND_WORKER` placement. This is a JS interface event, not a REST event: you cannot subscribe to it with a request to `/rest/`.

{% endnote %}

## What the Handler Receives

No data is passed to the event handler.

## Event Subscription Parameters

The handler is registered from the widget with the [BX24.placement.bindEvent](../../bx24-placement-bind-event.md) method.

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../../../data-types.md) | The name of the interface event.

For this event — `BackgroundCallCard::hangupButtonClick` ||
|| **callback***
[`callable`](../../../../data-types.md) | The function Bitrix24 invokes when the event occurs. No arguments are passed to the handler ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.bindEvent('BackgroundCallCard::hangupButtonClick', function () {
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

    await $b24.placement.bindEvent('BackgroundCallCard::hangupButtonClick', () => {
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

        await $b24.placement.bindEvent('BackgroundCallCard::hangupButtonClick', () => {
          // handler code
        })
      })
    </script>
    ```

{% endlist %}

## Errors

Check the following conditions.

- The widget is open in the `PAGE_BACKGROUND_WORKER` placement. In other placements, the `BackgroundCallCard::*` events are not registered, and the subscription silently fails
- The event name is passed without typos and with the correct capitalization. The list of events available in the current placement is returned by [BX24.placement.getInterface](../../bx24-placement-get-interface.md)
- The call was raised by the application with the [telephony.externalCall.register](../../../../telephony/telephony-external-call-register.md) method. For calls made by Bitrix24 itself, the `BackgroundCallCard::*` events are not emitted at all

## Continue Learning

- [{#T}](../../bx24-placement-bind-event.md)
- [{#T}](../call-card-close.md)
- [{#T}](./close-button-click.md)
- [{#T}](./index.md)
- [{#T}](../card.md)
- [{#T}](../index.md)
