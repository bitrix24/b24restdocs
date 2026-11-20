# Change the Call Card Interface State from the Application CallCardSetUiState

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can execute the command: an employee to whom Bitrix24 showed the call card, if they have access to the application

The `CallCardSetUiState` command changes the interface state of the call card.

The command works only with the call card of an external call, which the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method shows. For calls made by Bitrix24 itself, the command returns the `Call card is undefined` error in the browser. Call the command after the [BackgroundCallCard::initialized](./events/initialized.md) event: before it and after the card is closed, the same error is returned.

{% note info "" %}

The command operates within the application context in the `PAGE_BACKGROUND_WORKER` placement. This is a JS interface command, not a REST method: it cannot be invoked with a request to `/rest/`.

{% endnote %}

## How to Call the Command

The command is invoked from the widget with the [BX24.placement.call](../bx24-placement-call.md) method. The third argument is the callback function, which receives the result of the command.

```js
BX24.placement.call('CallCardSetUiState', {uiState: 'connected'}, function (result) {
    console.log(result);
});
```

## Command Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **uiState***
[`string`](../../../data-types.md) | The interface state of the card.

Possible values:

- `incoming` — incoming call
- `transferIncoming` — incoming call transfer
- `outgoing` — outgoing call
- `connectingIncoming` — connecting an incoming call
- `connectingOutgoing` — connecting an outgoing call
- `connected` — connection established
- `transferring` — the call is being transferred
- `transferFailed` — call transfer error
- `transferConnected` — the call transfer was connected successfully
- `error` — call error
- `moneyError` — an error caused by insufficient funds
- `redial` — redial

The same list can be retrieved programmatically with the [CallCardGetListUiStates](./call-card-get-list-ui-states.md) command. How the card looks in each state and which buttons are available in it is shown on the page [{#T}](./card.md).

The command also accepts, without an error, the internal values `idle`, `externalCard`, and `sipPhoneError`, which are not in the list. The `Invalid ui state` error is returned only for a value that is neither in the list nor among the internal values. In the `externalCard` state, or in the `idle` state for an incoming call, only the "Close" button remains at the bottom of the card ||
|| **disableAutoStartTimer**
[`boolean`](../../../data-types.md) | An additional parameter for `uiState = connected`.

Possible values:

- `true` — do not start the timer automatically
- `false` or no parameter — start the timer automatically ||
|#

Bitrix24 shows the external call card in the internal `externalCard` state: it has only the "Close" button at the bottom. After that, only the application switches the states: the card buttons send events to it but do not change the state. The exception is the "Close" button: Bitrix24 handles it itself and closes the card.

A typical incoming call goes like this:

1. `incoming` — the application shows the operator the call with the "Answer" and "Ignore" buttons
2. The operator clicks "Answer", the application receives the [BackgroundCallCard::answerButtonClick](./events/answer-button-click.md) event and connects the call in its WebRTC client. While the call is connecting, you can switch to `connectingIncoming` — it has the "Hang Up" button
3. `connected` — the conversation is in progress, and the timer starts automatically
4. The conversation is over — the application records the call with the [telephony.externalCall.finish](../../../telephony/telephony-external-call-finish.md) method and closes the card with the [CallCardClose](./call-card-close.md) command

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
            BX24.placement.call('CallCardSetUiState', {uiState: 'connected'}, function (result) {
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

    await $b24.placement.call('CallCardSetUiState', { uiState: 'connected' })
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        const result = await $b24.placement.call('CallCardSetUiState', {uiState: 'connected'})

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
        "errorCode": "Invalid ui state"
    }
]
```

### errorCode Values

#|
|| **Code** | **Description** | **Value** ||
|| `Call card is undefined` | The call card is unavailable | There is no external call card. The command was called:

- before the `BackgroundCallCard::initialized` event
- after the card was closed
- during a call that was not registered with the `telephony.externalCall.register` method, for example, a call made by Bitrix24 itself ||
|| `Invalid ui state` | An incorrect `uiState` value | The passed value is neither in the list of supported states nor among the internal values `idle`, `externalCard`, and `sipPhoneError` ||
|#

If the command is invoked in another placement, the callback function will not be invoked at all: the placement interface ignores an unknown command. To check that the `CallCardSetUiState` command is available, use the [BX24.placement.getInterface](../bx24-placement-get-interface.md) method.

## Continue Learning

- [{#T}](./call-card-set-mute.md)
- [{#T}](./call-card-set-hold.md)
- [{#T}](./call-card-get-list-ui-states.md)
- [{#T}](./call-card-set-card-title.md)
- [{#T}](./call-card-set-status-text.md)
- [{#T}](./call-card-close.md)
- [{#T}](./events/index.md)
- [{#T}](./index.md)
