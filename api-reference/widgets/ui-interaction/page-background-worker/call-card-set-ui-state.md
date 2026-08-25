# Change the Call Card Interface State from the Application CallCardSetUiState

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can execute the command: any user

The `CallCardSetUiState` command changes the interface state of the call card.

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

The same list can be retrieved programmatically with the [CallCardGetListUiStates](./call-card-get-list-ui-states.md) command. How the card looks in each state and which buttons are available in it is shown on the page [{#T}](./card.md) ||
|| **disableAutoStartTimer**
[`boolean`](../../../data-types.md) | An additional parameter for `uiState = connected`.

Possible values:

- `true` — do not start the timer automatically
- `false` or no parameter — start the timer automatically ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

It is recommended to call the command after the [BackgroundCallCard::initialized](./events/initialized.md) event

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
|| `Call card is undefined` | The call card is unavailable | There is no active call card to manage ||
|| `Invalid ui state` | An incorrect `uiState` value | The state that was passed is not in the list of supported states ||
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
