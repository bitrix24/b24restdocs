# When Selecting an Operator to Transfer the Call To BackgroundCallCard::transferButtonClick

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`](../../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can subscribe: any user

The `BackgroundCallCard::transferButtonClick` event occurs when the operator clicks the transfer button in the call card and selects the employee to transfer the call to.

The transfer button is available only in the `connected` state, which the application enables with the [CallCardSetUiState](../call-card-set-ui-state.md) command. In call list mode and in the card of a call that was itself transferred, the button is not shown.

This is the first step of the transfer scenario. Bitrix24 does not transfer an application call itself. The application receives the recipient, connects them, and switches the card to the `transferring` state with the same command — in this state the operator sees the "Redirect" and "Continue call" buttons. Clicks on these buttons arrive as the [completeTransferButtonClick](./complete-transfer-button-click.md) and [cancelTransferButtonClick](./cancel-transfer-button-click.md) events.

{% note info "" %}

The event operates within the application context in the `PAGE_BACKGROUND_WORKER` placement. This is a JS interface event, not a REST event: you cannot subscribe to it with a request to `/rest/`.

{% endnote %}

## What the Handler Receives

Data is passed to the callback `BX24.placement.bindEvent` {.b24-info}

```js
callback({
    "phoneNumber": "+19001234567",
    "target": 12
});
```

## Event Handler Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **phoneNumber**
[`string`](../../../../data-types.md) | The phone number of the other party ||
|| **target**
[`integer`](../../../../data-types.md) or [`string`](../../../../data-types.md) | Where the call is being transferred to.

The value depends on the item the operator selected:

- the employee ID as a number, for example `12`, — if the employee's profile has no phone numbers and no selection menu is shown
- the employee ID as a string, for example `"12"`, — if the operator selected the "Internal call" item in the menu
- a phone number as a string, for example `"+19007654321"`, — mobile, personal, or work number from the employee's profile, if the operator selected it in the menu

The transfer type — to an employee or to a phone — is not passed to the handler. The application determines it from the value: an employee ID matches the `ID` from [user.get](../../../../user/user-get.md), and a phone number matches the employee's `PERSONAL_MOBILE`, `PERSONAL_PHONE`, or `WORK_PHONE` field.

If the operator selected a department rather than an employee, the event does not occur ||
|#

## Event Subscription Parameters

The handler is registered from the widget with the [BX24.placement.bindEvent](../../bx24-placement-bind-event.md) method.

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../../../data-types.md) | The name of the interface event.

For this event — `BackgroundCallCard::transferButtonClick` ||
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
            BX24.placement.bindEvent('BackgroundCallCard::transferButtonClick', function (eventData) {
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

    await $b24.placement.bindEvent('BackgroundCallCard::transferButtonClick', (eventData: { phoneNumber: string; target: number | string }) => {
      console.log(eventData.target)
    })
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        await $b24.placement.bindEvent('BackgroundCallCard::transferButtonClick', (eventData) => {
          console.log(eventData)
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

- [{#T}](./index.md)
- [{#T}](../../bx24-placement-bind-event.md)
- [{#T}](../card.md)
- [{#T}](../index.md)
- [{#T}](./complete-transfer-button-click.md)
- [{#T}](./cancel-transfer-button-click.md)
- [{#T}](../call-card-set-ui-state.md)
