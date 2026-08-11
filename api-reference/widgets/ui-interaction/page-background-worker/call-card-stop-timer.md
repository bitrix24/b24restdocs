# Stop the Call Timer from the Application CallCardStopTimer

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can execute the command: any user

The `CallCardStopTimer` command stops counting the conversation time in the call card.

{% note info "" %}

The command operates within the application context in the `PAGE_BACKGROUND_WORKER` placement. This is a JS interface command, not a REST method: it cannot be invoked with a request to `/rest/`.

{% endnote %}

The command is needed when the application shows its own conversation time in the card or puts the call on hold. When the card switches to a state other than `connected`, the timer stops on its own — see [CallCardSetUiState](./call-card-set-ui-state.md).

## How to Call the Command

The command is invoked from the widget with the [BX24.placement.call](../bx24-placement-call.md) method. The third argument is the callback function, which receives the result of the command.

```js
BX24.placement.call('CallCardStopTimer', {}, function (result) {
    // in the browser, the callback function is not invoked on success
    console.log(result);
});
```

## Command Parameters

The command takes no parameters. Pass an empty object `{}` as the second argument.

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
            BX24.placement.call('CallCardStopTimer', {}, function (result) {
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

    // in the browser, the promise does not resolve on success — do not await the result
    $b24.placement.call('CallCardStopTimer')
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        // in the browser, the promise does not resolve on success — do not await the result
        $b24.placement.call('CallCardStopTimer')
      })
    </script>
    ```

{% endlist %}

## Command Result

In the browser, a successful call returns nothing: the callback function is not invoked, and the counting in the card stops.

In the Bitrix24 desktop application, an empty array arrives after a successful call.

```json
[]
```

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
|| `Call card is undefined` | The call card is unavailable | There is no active call card to manage. The card is raised by the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method ||
|#

If the command is invoked in another placement, the callback function will not be invoked at all: the placement interface ignores an unknown command. To check that the `CallCardStopTimer` command is available, use the [BX24.placement.getInterface](../bx24-placement-get-interface.md) method.

## Continue Learning

- [{#T}](./call-card-start-timer.md)
- [{#T}](./call-card-set-ui-state.md)
- [{#T}](./card.md)
- [{#T}](./events/index.md)
- [{#T}](./index.md)
