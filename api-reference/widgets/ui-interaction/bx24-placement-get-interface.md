# Get Information About the JS Interface of the Current Placement BX24.placement.getInterface

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `BX24.placement.getInterface` retrieves information about the JS interface of the current placement: the list of available commands and events.

```js
BX24.placement.getInterface(callback);
```

Each placement has its own set of commands and events. Check it with this method before calling [BX24.placement.call](bx24-placement-call.md) and before subscribing through [BX24.placement.bindEvent](bx24-placement-bind-event.md).

Bitrix24 silently ignores an unknown command and a subscription to an unregistered event, without raising an error.

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **callback***
[`callable`](../../data-types.md) | Callback function. It receives an object with the fields `command` and `event` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.getInterface(function (result) {
                console.info(result.command, result.event);
            });
        });
    });
    ```

- JS (TS)

    ```ts
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide)
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PlacementInterface = {
      command: string[]
      event: string[]
    }

    const result = await $b24.placement.getInterface() as PlacementInterface

    console.info(result.command, result.event)
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        const result = await $b24.placement.getInterface()

        console.info(result.command, result.event)
      })
    </script>
    ```

{% endlist %}

## Result

An example for a widget opened in the `CALL_CARD` placement.

```json
{
    "command": ["getStatus", "disableAutoClose", "enableAutoClose"],
    "event": ["CallCard::EntityChanged", "CallCard::BeforeClose", "CallCard::CallStateChanged"]
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **command**
[`string[]`](../../data-types.md) | The names of the commands registered by the current placement.

General widget methods — for example [resizeWindow](../../../sdk/bx24-js-sdk/additional-functions/bx24-resize-window.md) — are not included in this list, although they can still be called ||
|| **event**
[`string[]`](../../data-types.md) | The names of the events you can subscribe to in the current placement. If the placement has no events of its own, the array is empty ||
|#

## Errors

The method returns no errors. Empty `command` and `event` arrays mean that the current placement has no commands and events of its own — for example, the widget was opened through the main application link.

## Continue Learning

- [{#T}](bx24-placement-info.md)
- [{#T}](bx24-placement-call.md)
- [{#T}](bx24-placement-bind-event.md)
- [{#T}](index.md)
