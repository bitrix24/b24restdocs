# Toggle the Microphone Button in the Call Card from the Application CallCardSetMute

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can execute the command: an employee to whom Bitrix24 showed the call card, if they have access to the application

The `CallCardSetMute` command toggles the operator's microphone button in the call card: it shows that the microphone is muted or unmuted.

The microphone can be muted in the card or in the application. If the operator clicks the microphone button in the card, the application receives the [BackgroundCallCard::muteButtonClick](./events/mute-button-click.md) event and mutes the microphone in its WebRTC client. If the microphone was muted in the application, toggle the button in the card with this command. The command toggles only the button and does not trigger the `muteButtonClick` event.

The command works only with the call card of an external call, which the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method shows. For calls made by Bitrix24 itself, the command returns the `Call card is undefined` error in the browser. Call the command after the [BackgroundCallCard::initialized](./events/initialized.md) event: before it and after the card is closed, the same error is returned.

{% note info "" %}

The command operates within the application context in the `PAGE_BACKGROUND_WORKER` placement. This is a JS interface command, not a REST method: it cannot be invoked with a request to `/rest/`.

{% endnote %}

## How to Call the Command

The command is invoked from the widget with the [BX24.placement.call](../bx24-placement-call.md) method. The third argument is the callback function, which receives the result of the command.

```js
BX24.placement.call('CallCardSetMute', {muted: true}, function (result) {
    console.log(result);
});
```

## Command Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **muted***
[`boolean`](../../../data-types.md) | Microphone control.

Possible values:

- `true` — show in the card that the microphone is muted
- `false` — show in the card that the microphone is unmuted

In the browser, without the parameter, the command unmutes the microphone, as with `false`. In the Bitrix24 desktop application, without the parameter, the `missing field muted` error is returned ||
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
            BX24.placement.call('CallCardSetMute', {muted: true}, function (result) {
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

    await $b24.placement.call('CallCardSetMute', { muted: true })
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        const result = await $b24.placement.call('CallCardSetMute', {muted: true})

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

If the microphone is already in the requested state, the command returns an empty array right away and does not change the card.

## Errors

The command error arrives in the same callback function. Usually, it is an array with an object whose `result` equals `error`. The `missing field muted` error arrives as a single object, without an array. Therefore, check the `result` field both in the object and in the first element of the array: the `error` value means that the command was not executed.

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
|| `missing field muted` | The required `muted` parameter was not passed | In the Bitrix24 desktop application, the `muted` field is required. This code arrives as a single object, without being wrapped in an array ||
|#

If the command is invoked in another placement, the callback function will not be invoked at all: the placement interface ignores an unknown command. To check that the `CallCardSetMute` command is available, use the [BX24.placement.getInterface](../bx24-placement-get-interface.md) method.

## Continue Learning

- [{#T}](./call-card-set-hold.md)
- [{#T}](./call-card-set-ui-state.md)
- [{#T}](./call-card-get-list-ui-states.md)
- [{#T}](./call-card-set-card-title.md)
- [{#T}](./call-card-set-status-text.md)
- [{#T}](./call-card-close.md)
- [{#T}](./events/index.md)
- [{#T}](./index.md)
