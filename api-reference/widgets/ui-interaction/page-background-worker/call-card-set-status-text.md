# Change the Status Text in the Call Card from the Application CallCardSetStatusText

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can execute the command: an employee to whom Bitrix24 showed the call card, if they have access to the application

The `CallCardSetStatusText` command changes the status text in the call card — the line below the client details. It usually shows the operator what is happening with the call in the application, for example, "Connecting to the client". The title in the top line of the card is changed by the [CallCardSetCardTitle](./call-card-set-card-title.md) command.

In the initial state of the card, the status line is hidden. The text appears when the application switches the card to a working state with the [CallCardSetUiState](./call-card-set-ui-state.md) command, for example, `incoming` or `connected`.

Bitrix24 does not write its own status to the external call card. The application's text is retained until the next command call or until the card is closed; changing the state does not reset it.

The command works only with the call card of an external call, which the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method shows. For calls made by Bitrix24 itself, the command returns the `Call card is undefined` error in the browser. Call the command after the [BackgroundCallCard::initialized](./events/initialized.md) event: before it and after the card is closed, the same error is returned.

{% note info "" %}

The command operates within the application context in the `PAGE_BACKGROUND_WORKER` placement. This is a JS interface command, not a REST method: it cannot be invoked with a request to `/rest/`.

{% endnote %}

## How to Call the Command

The command is invoked from the widget with the [BX24.placement.call](../bx24-placement-call.md) method. The third argument is the callback function, which receives the result of the command.

```js
BX24.placement.call('CallCardSetStatusText', {statusText: 'Connection established'}, function (result) {
    console.log(result);
});
```

## Command Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **statusText***
[`string`](../../../data-types.md) | The new status text below the client details ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The examples show the command call itself. In an application, it is usually placed in the [BackgroundCallCard::initialized](./events/initialized.md) event handler.

{% endnote %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.call('CallCardSetStatusText', {statusText: 'Connection established'}, function (result) {
                console.log(result);
            });
        });
    });
    ```

- JS (TS)

    ```ts
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide)
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    await $b24.placement.call('CallCardSetStatusText', { statusText: 'Connection established' })
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        const result = await $b24.placement.call('CallCardSetStatusText', {statusText: 'Connection established'})

        console.log(result)
      })
    </script>
    ```

{% endlist %}

## Command Result

```json
[]
```

### Returned Data

An empty array on a successful call.

## Errors

The command error arrives in the same callback function: instead of the usual result, it receives an array with an object whose `result` equals `error`.

```json
[
    {
        "result": "error",
        "errorCode": "Call card is undefined"
    }
]
```

### errorCode Values

#|
|| **Code** | **Description** | **Value** ||
|| `Call card is undefined` | The call card is unavailable | There is no external call card. The command was called:

- before the `BackgroundCallCard::initialized` event
- after the card was closed
- during a call that was not registered with the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method, for example, a call made by Bitrix24 itself ||
|| `missing field statusText` | The required `statusText` parameter was not passed | In the Bitrix24 desktop application, the `statusText` field is required ||
|#

If the command is invoked in another placement, the callback function will not be invoked at all: the placement interface ignores an unknown command. To check that the `CallCardSetStatusText` command is available, use the [BX24.placement.getInterface](../bx24-placement-get-interface.md) method.

## Continue Learning

- [{#T}](./call-card-set-card-title.md)
- [{#T}](./call-card-set-mute.md)
- [{#T}](./call-card-set-hold.md)
- [{#T}](./call-card-set-ui-state.md)
- [{#T}](./call-card-get-list-ui-states.md)
- [{#T}](./call-card-close.md)
- [{#T}](./events/index.md)
- [{#T}](./index.md)
