# Start the Call Timer from the Application CallCardStartTimer

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can execute the command: an employee to whom Bitrix24 showed the call card, if they have access to the application

The `CallCardStartTimer` command shows the timer in the call card and starts counting the conversation time.

The command works only with the call card of an external call, which the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method shows. For calls made by Bitrix24 itself, the command returns the `Call card is undefined` error in the browser. Call the command after the [BackgroundCallCard::initialized](./events/initialized.md) event: before it and after the card is closed, the same error is returned.

{% note info "" %}

The command operates within the application context in the `PAGE_BACKGROUND_WORKER` placement. This is a JS interface command, not a REST method: it cannot be invoked with a request to `/rest/`.

{% endnote %}

The command is needed when the application counts the conversation time itself. When the card switches to the `connected` state, the timer starts automatically — the automatic start can be disabled with the `disableAutoStartTimer` parameter of the [CallCardSetUiState](./call-card-set-ui-state.md) command.

The countdown runs from the moment of the first start, not from the command call. If the timer is already running, the command changes nothing. If the timer was stopped with the [CallCardStopTimer](./call-card-stop-timer.md) command, `CallCardStartTimer` starts it again and shows the entire time since the first start, including the time while the timer was stopped.

To start counting from zero, stop the timer and switch the card to any state other than `connected`. The timer is hidden and reset. On the next switch to `connected`, the timer starts counting again from zero. If you do not need the automatic start, switch the card to `connected` with `disableAutoStartTimer: true` and call `CallCardStartTimer`.

## How to Call the Command

The command is invoked from the widget with the [BX24.placement.call](../bx24-placement-call.md) method. The third argument is the callback function, which receives the result of the command.

```js
BX24.placement.call('CallCardStartTimer', {}, function (result) {
    // in the browser, the callback function is not invoked on success
    console.log(result);
});
```

## Command Parameters

The command takes no parameters. In BX24.js, pass an empty object `{}` as the second argument; in B24JsSDK, the second argument can be omitted.

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
            BX24.placement.call('CallCardStartTimer', {}, function (result) {
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

    // on success, the command sends no response — do not wait for it
    $b24.placement.call('CallCardStartTimer')
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        // on success, the command sends no response — do not wait for it
        $b24.placement.call('CallCardStartTimer')
      })
    </script>
    ```

{% endlist %}

## Command Result

In the browser, a successful call returns nothing: the callback function is not invoked, and the timer appears in the card.

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
|| `Call card is undefined` | The call card is unavailable | There is no active call card to manage. The same code arrives when the card exists but the call is not an external one: the interface only works with a card raised by the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method ||
|#

If the command is invoked in another placement, the callback function will not be invoked at all: the placement interface ignores an unknown command. To check that the `CallCardStartTimer` command is available, use the [BX24.placement.getInterface](../bx24-placement-get-interface.md) method.

## Continue Learning

- [{#T}](./call-card-stop-timer.md)
- [{#T}](./call-card-set-ui-state.md)
- [{#T}](./card.md)
- [{#T}](./events/index.md)
- [{#T}](./index.md)
