# Interaction with UI: Overview of Methods

This section describes client methods and scenarios through which the application interacts with the Bitrix24 interface without making REST requests to the server. The methods operate within the context of the current placement and exchange data via `postMessage`.

Through this section, you can obtain the context of the current placement, learn about the list of available commands and events, subscribe to interface events, and invoke UI commands. Specialized interfaces with additional commands and events are available for certain placements.

> Quick Navigation: [All Methods](#all-methods)

## Getting Started

1. Open the application in the desired placement.
2. Obtain the call context via [BX24.placement.info](./bx24-placement-info.md).
3. Request available commands and events through [BX24.placement.getInterface](./bx24-placement-get-interface.md).
4. Subscribe to events via [BX24.placement.bindEvent](./bx24-placement-bind-event.md) and invoke commands through [BX24.placement.call](./bx24-placement-call.md).
5. If you need to open a standard Bitrix24 page, a separate application slider, or a system dialog, use [additional methods of BX24.js](../bx24-widget-methods.md) and [system dialogues](../../../sdk/bx24-js-sdk/system-dialogues/index.md).

## Limitations and Features

- The methods in this section operate on the interface side and do not replace the REST API.
- Before invoking special commands, it is advisable to first check the available interface via [BX24.placement.getInterface](./bx24-placement-get-interface.md).
- The interface command works only where it is supported. The same call may be valid for `CALL_CARD` and unavailable in another placement.
- The methods [BX24.openPath](../bx24-widget-methods.md), [BX24.openApplication](../bx24-widget-methods.md), and [BX24.closeApplication](../bx24-widget-methods.md) manage navigation and interface windows but do not replace data retrieval or storage via the REST API.

## Special Features of Individual Placements

Some placements support not only the basic methods `BX24.placement.*` but also a special JS interface for managing specific elements of Bitrix24.

#|
|| **Placement** | **What is Available** ||
|| [Call Card `CALL_CARD`](./call-card/index.md) | Methods for obtaining call status, managing auto-closing of the card, and call card events. ||
|| [CRM Card](#crm-card) | Additional interface command `reloadData` for updating form data. ||
|| [Background Placement `PAGE_BACKGROUND_WORKER`](./page-background-worker/index.md) | Call card events and methods for managing its interface from background embedding. ||
|#

## CRM Card {#crm-card}

In the context of the CRM card, an additional interface command `reloadData` is available. It is invoked via [BX24.placement.call](./bx24-placement-call.md) to refresh the form data in the interface after application actions.

```js
BX24.placement.call('reloadData', {}, function () {
    console.log('reloadData called');
});
```

The command updates data only in the card interface. It does not automatically save changes in the CRM. To ensure the data is stored in the database, the user must save the card.

Before invoking the command, it is recommended to check the available interface via [BX24.placement.getInterface](./bx24-placement-get-interface.md).

## Overview of Methods {#all-methods}

> Scope: [`placement`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [BX24.placement.info](./bx24-placement-info.md) | Retrieves information about the call context. ||
|| [BX24.placement.getInterface](./bx24-placement-get-interface.md) | Retrieves a list of available commands and events for the current placement. ||
|| [BX24.placement.call](./bx24-placement-call.md) | Invokes a registered interface command. ||
|| [BX24.placement.bindEvent](./bx24-placement-bind-event.md) | Registers an event handler for the interface. ||
|#