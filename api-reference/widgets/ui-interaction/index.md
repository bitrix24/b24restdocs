# Interaction with UI: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The `BX24.placement.*` methods give a widget access to the Bitrix24 interface: they return the call context, show the available commands and events, invoke commands, and register handlers. They operate in the placement where the widget is open and exchange data with the Bitrix24 window via `postMessage`.

{% note warning "" %}

These are methods of a JS library, not of the REST API. They are called in the browser from the widget code — through [BX24.js](../../../sdk/bx24-js-sdk/index.md) or the [b24jssdk](../../../sdk/b24jssdk/index.md) library. The REST methods `placement.call` and `placement.bindEvent` do not exist: a request to `/rest/` with such names returns an error. Through REST, you only register the placement itself, with the [placement.bind](../placement-bind.md) method.

{% endnote %}

Some placements have commands and events of their own — the call card, for example. They are described in the child sections.

> Quick navigation: [all methods](#all-methods)

## Getting Started

1. Register the widget with the [placement.bind](../placement-bind.md) method and open it in the desired placement
2. Connect a client library in the widget code. [BX24.js](../../../sdk/bx24-js-sdk/index.md) is connected with a single script and works through callback functions, while [b24jssdk](../../../sdk/b24jssdk/index.md) is installed as a package and returns promises. Without a library, there is no `BX24` object on the page
3. Retrieve the call context with the [BX24.placement.info](./bx24-placement-info.md) method — it shows which placement the widget is open in and which parameters arrived with it
4. Request the available commands and events with the [BX24.placement.getInterface](./bx24-placement-get-interface.md) method — the set depends on the placement
5. Subscribe to events with the [BX24.placement.bindEvent](./bx24-placement-bind-event.md) method and invoke commands with the [BX24.placement.call](./bx24-placement-call.md) method
6. If you need to open a standard Bitrix24 page, a separate application slider, or a system dialog, use the [additional methods of BX24.js](../bx24-widget-methods.md) and the [system dialogues](../../../sdk/bx24-js-sdk/system-dialogues/index.md)

```js
BX24.ready(function () {
    BX24.init(function () {
        BX24.placement.getInterface(function (result) {
            // result.command — available commands, result.event — available events
            console.log(result);
        });
    });
});
```

## Limitations and Features

- Each placement has its own set of commands and events. The same call may be valid for `CALL_CARD` and not exist elsewhere in the interface
- The placement interface silently ignores an unknown command and a subscription to an unregistered event: the callback function is not invoked and no error arrives. That is why you should check the interface with the [BX24.placement.getInterface](./bx24-placement-get-interface.md) method before making a call
- The methods of this section operate on the interface side and do not replace the REST API: data still has to be retrieved and retained with REST methods. This also applies to the navigation methods [BX24.openPath](../bx24-widget-methods.md), [BX24.openApplication](../bx24-widget-methods.md), and [BX24.closeApplication](../bx24-widget-methods.md)

## Special Features of Individual Placements

Some placements support not only the basic `BX24.placement.*` methods but also commands and events of their own for managing specific Bitrix24 elements.

#|
|| **Placement** | **What is available** ||
|| [Call card `CALL_CARD`](./call-card/index.md) | Data of the current call, control over the auto-closing of the card, and call card events ||
|| [CRM card, activity, and list](#crm-card) | The `reloadData` command for refreshing data in the CRM interface ||
|| [Activity in the CRM timeline](../crm/detail-activity-area.md) | Commands for rendering the standard activity interface: `setLayout`, `setLayoutItemState`, `setPrimaryButtonState`, `setSecondaryButtonState`, `bindLayoutEventCallback`, `bindValueChangeCallback`, `lock`, `unlock`, `finish` ||
|| [Client search in the CRM card](../crm/detail-search.md) | The `crmShowFoundEntities` and `crmShowCreatedEntity` commands and the `onCrmEntityIsNeedToCreate` event ||
|| [Requisite autocomplete](../crm/requisites-autocomplete/requisite-autocomplete.md) | The `crmShowFoundEntities` and `crmShowCreatedEntity` commands and the `onCrmEntityIsNeedToCreate` event ||
|| [Background placement `PAGE_BACKGROUND_WORKER`](./page-background-worker/index.md) | Control over the call card interface from a background widget and events of the operator's actions ||
|| [Calendar `CALENDAR_GRIDVIEW`](../../calendar/calendar-grid-view.md) | The `getEvents`, `viewEvent`, `addEvent`, `editEvent`, `deleteEvent` commands and the `Calendar.customView:*` events — refreshing the list and changing the period ||
|| [Custom field type `USERFIELD_TYPE`](../user-field/index.md) | The `setValue` and `getValue` commands for reading and writing the field value. For a usage example, see the tutorial [{#T}](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) ||
|| [Block on a website](../../landing/embedding/block.md) | The `refreshBlock` command for redrawing the block after the application makes changes ||
|| [Settings of a robot application](../../../tutorials/bizproc/setting-robot.md) | The `setPropertyValue` command for writing property values into the robot form ||
|#

Besides the commands of the placement, general methods are available to every widget — for example [BX24.resizeWindow](../../../sdk/bx24-js-sdk/additional-functions/bx24-resize-window.md), which changes the frame size. They are not returned in the list from [BX24.placement.getInterface](./bx24-placement-get-interface.md).

## Refreshing Data in the CRM {#crm-card}

The `reloadData` command refreshes data in the CRM interface after the application performs an action. It is invoked through [BX24.placement.call](./bx24-placement-call.md) and is available to widgets of three placement families:

- `CRM_*_DETAIL_TAB` — a tab of the item card, the card form is refreshed
- `CRM_*_DETAIL_ACTIVITY` — an activity in the card timeline, the same card is refreshed
- `CRM_*_LIST_MENU` — a menu item above the item list, the list is refreshed

```js
BX24.placement.call('reloadData', {}, function () {
    console.log('reloadData called');
});
```

The command refreshes data only in the interface and invokes the callback function without arguments. It does not retain changes in the CRM: for the data to reach the database, the user has to save the card.

## Overview of Methods {#all-methods}

> Scope: [`placement`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **When it is needed** ||
|| [BX24.placement.info](./bx24-placement-info.md) | To find out which placement the widget is open in and which parameters arrived with it ||
|| [BX24.placement.getInterface](./bx24-placement-get-interface.md) | To check whether the current placement supports the command or event you need ||
|| [BX24.placement.call](./bx24-placement-call.md) | To perform an action in the interface: refresh a form, change the call card, write a value into a field ||
|| [BX24.placement.bindEvent](./bx24-placement-bind-event.md) | To react to user actions in the Bitrix24 interface ||
|#

## Continue Learning

- [{#T}](./call-card/index.md)
- [{#T}](./page-background-worker/index.md)
- [{#T}](../placement-bind.md)
- [{#T}](../index.md)
