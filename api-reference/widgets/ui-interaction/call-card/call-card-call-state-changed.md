# Call Status Change Event CallCard::CallStateChanged

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — access to the call card placement
>
> Who can subscribe: any user

The `CallCard::CallStateChanged` event occurs when the state of the current call changes.

{% note info "" %}

The event operates within the application context in the `CALL_CARD` placement. This is a JS interface event, not a REST event: you cannot subscribe to it with a request to `/rest/`.

{% endnote %}

## What the Handler Receives

Data is passed to the callback `BX24.placement.bindEvent` {.b24-info}

```js
callback(
    "idle",
    {
        "failedCode": "486"
    }
);
```

## Event Handler Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **callState***
[`string`](../../../data-types.md) | The current state of the call.

Possible values:

- `idle` — no connection
- `connecting` — establishing connection
- `connected` — connection established ||
|| **additionalParams**
[`object`](../../../data-types.md) | Additional data [(detailed description)](#additional_params).

The argument always arrives: if there is no additional data, it is an empty object. The asterisk marks the required subscription parameters, not the handler arguments ||
|#

### additionalParams Parameter {#additional_params}

#|
|| **Parameter**
`type` | **Description** ||
|| **failedCode**
[`string`](../../../data-types.md) | The call termination code. It is passed only on an unsuccessful termination, when `callState = idle`.

The value is a SIP protocol code. For example, `486` — the subscriber is busy, `480` — the subscriber is unavailable ||
|#

## Event Subscription Parameters

The handler is registered from the widget with the [BX24.placement.bindEvent](../bx24-placement-bind-event.md) method.

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | The name of the interface event.

For this event — `CallCard::CallStateChanged` ||
|| **callback***
[`callable`](../../../data-types.md) | The function Bitrix24 invokes when the event occurs. The handler arguments are described above ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.bindEvent('CallCard::CallStateChanged', function (callState, additionalParams) {
                console.log(callState);
            });
        });
    });
    ```

- JS (TS)

    ```ts
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide)
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    await $b24.placement.bindEvent('CallCard::CallStateChanged', (callState: string, additionalParams: { failedCode?: string }) => {
      console.log(callState, additionalParams.failedCode)
    })
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        await $b24.placement.bindEvent('CallCard::CallStateChanged', (callState, additionalParams) => {
          console.log(callState)
        })
      })
    </script>
    ```

{% endlist %}

## Errors

Check the following conditions.

- The widget is open in the `CALL_CARD` placement. In other placements, the `CallCard::*` events are not registered, and the subscription silently fails
- The call state has actually changed. Setting the same `callState` value again does not trigger the event
- The event name is passed without typos and with the correct capitalization. The list of events available in the current placement is returned by [BX24.placement.getInterface](../bx24-placement-get-interface.md)

## Continue Learning

- [{#T}](./get-status.md)
- [{#T}](./disable-auto-close.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-entity-changed.md)
- [{#T}](./call-card-before-close.md)
- [{#T}](./index.md)
- [{#T}](../../telephony/call-card.md)
