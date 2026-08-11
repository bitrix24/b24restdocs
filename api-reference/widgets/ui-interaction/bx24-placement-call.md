# Call the Registered Interface Command BX24.placement.call

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `BX24.placement.call` invokes an interface command registered by the current placement.

```js
BX24.placement.call(command, parameters[, callback]);
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **command***
[`string`](../../data-types.md) | The name of the command to invoke ||
|| **parameters**
[`any`](../../data-types.md) | Passed parameters. The value type depends on the command: object, string, number, array, or `null`. Commands without parameters accept an empty object `{}` ||
|| **callback**
[`callable`](../../data-types.md) | Callback function. It receives the result of the command ||
|#

For example, the command `setValue` of the `USERFIELD_TYPE` placement accepts the new field value as its second argument:

```js
BX24.placement.call('setValue', value, () => {});
```

## Available Commands

The set of commands is defined by the placement where the widget is open. There is no single list covering all placements: check it with [BX24.placement.getInterface](bx24-placement-get-interface.md).

In the b24jssdk library, the second argument is optional: omit it for commands without parameters.

#|
|| **Placement** | **Commands** ||
|| [`CALL_CARD`](./call-card/index.md) | `getStatus`, `disableAutoClose`, `enableAutoClose` ||
|| [`PAGE_BACKGROUND_WORKER`](./page-background-worker/index.md) | `CallCardSetMute`, `CallCardSetHold`, `CallCardSetUiState`, `CallCardGetListUiStates`, `CallCardSetCardTitle`, `CallCardSetStatusText`, `CallCardStartTimer`, `CallCardStopTimer`, `CallCardClose` ||
|| [`CALENDAR_GRIDVIEW`](../../calendar/calendar-grid-view.md) | `getEvents`, `viewEvent`, `addEvent`, `editEvent`, `deleteEvent` ||
|| [`CRM_*_DETAIL_TAB`, `CRM_*_DETAIL_ACTIVITY`, `CRM_*_LIST_MENU`](./index.md#crm-card) | `reloadData` — refreshes the CRM item card or the item list ||
|| [Activity in the CRM timeline](../crm/detail-activity-area.md) | `setLayout`, `setLayoutItemState`, `setPrimaryButtonState`, `setSecondaryButtonState`, `bindLayoutEventCallback`, `bindValueChangeCallback`, `lock`, `unlock`, `finish` — rendering of the standard activity interface ||
|| [Client search and requisite autocomplete](../crm/detail-search.md) | `crmShowFoundEntities`, `crmShowCreatedEntity` — pass the found options and the created CRM object ||
|| [`USERFIELD_TYPE`](../user-field/index.md) | `setValue`, `getValue`. For a usage example, see the tutorial [{#T}](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) ||
|| [Block on a website](../../landing/embedding/block.md) | `refreshBlock` — redraws the block after the application makes changes ||
|| [Settings of a robot application](../../../tutorials/bizproc/setting-robot.md) | `setPropertyValue` — writes property values into the robot form ||
|#

Besides the commands of the placement, general methods are available to the widget — for example [BX24.resizeWindow](../../../sdk/bx24-js-sdk/additional-functions/bx24-resize-window.md). They are not returned in the list from [BX24.placement.getInterface](bx24-placement-get-interface.md).

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.call('getStatus', {}, function (result) {
                console.log(result);
            });

            BX24.placement.call('CallCardSetCardTitle', {title: 'hello world!'}, function (result) {
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

    const result = await $b24.placement.call('getStatus')

    console.log(result)
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        const result = await $b24.placement.call('getStatus')

        console.log(result)
      })
    </script>
    ```

{% endlist %}

## Result

The content of the result is defined by the command itself and described on the command page. Commands that return nothing pass an empty array to the callback function, while read commands pass an object or an array with data.

A command returns its error to the same place — the JS interface has no separate error channel. For example, when there is no call card, the call card commands pass an array with an object:

```json
[
    {
        "result": "error",
        "errorCode": "Call card is undefined"
    }
]
```

## Errors

A placement silently ignores an unknown command: the callback function is not invoked and no error arrives. A call made from a placement where the command does not exist behaves the same way.

Check two conditions:

- the widget is open in the placement where the command is registered. The current placement is returned by [BX24.placement.info](bx24-placement-info.md)
- the command name is passed without typos and with the correct capitalization. The list of available commands is returned by [BX24.placement.getInterface](bx24-placement-get-interface.md)

## Continue Learning

- [{#T}](bx24-placement-info.md)
- [{#T}](bx24-placement-get-interface.md)
- [{#T}](bx24-placement-bind-event.md)
- [{#T}](index.md)
