# Set Up an Event Handler for the Interface BX24.placement.bindEvent

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `BX24.placement.bindEvent` sets an event handler for the interface. The event must be registered by the current placement, otherwise the subscription will not work.

```js
BX24.placement.bindEvent(event, callback);
```

These are interface events, not [REST events](../../events/index.md): the handler runs in the browser, in the widget code, and it is not registered with the `event.bind` method.

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | The name of the event to which the handler subscribes ||
|| **callback***
[`callable`](../../data-types.md) | Callback function.

The `callback` handler may or may not receive data depending on the event it subscribes to ||
|#

You need to subscribe to each event separately: there is no subscription to all events at once.

## Available Events

The set of events is defined by the placement where the widget is open.

#|
|| **Placement** | **Events** ||
|| [`CALL_CARD`](./call-card/index.md) | `CallCard::EntityChanged`, `CallCard::BeforeClose`, `CallCard::CallStateChanged` ||
|| [`PAGE_BACKGROUND_WORKER`](./page-background-worker/events/index.md) | 17 `BackgroundCallCard::*` events — from `initialized` to the operator's button clicks ||
|| [`CALENDAR_GRIDVIEW`](../../calendar/calendar-grid-view.md) | `Calendar.customView:refreshEntries`, `Calendar.customView:decreaseViewRangeDate`, `Calendar.customView:increaseViewRangeDate`, `Calendar.customView:adjustToDate` ||
|| [Client search and requisite autocomplete](../crm/detail-search.md) | `onCrmEntityIsNeedToCreate` — the user selected an option offered by the application ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.bindEvent('BackgroundCallCard::initialized', function (eventData) {
                console.log(eventData);
            });

            BX24.placement.bindEvent('CallCard::CallStateChanged', function (callState) {
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

    await $b24.placement.bindEvent('CallCard::CallStateChanged', (callState: string) => {
      console.log(callState)
    })
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        await $b24.placement.bindEvent('CallCard::CallStateChanged', (callState) => {
          console.log(callState)
        })
      })
    </script>
    ```

{% endlist %}

## Result

The method returns nothing. Data arrives in `callback` every time the event occurs — the set of arguments is described on the page of the specific event. A handler cannot return a value back to Bitrix24: events are one-way, and the application reacts with a separate command call or REST method call.

## Errors

Bitrix24 silently ignores a subscription to an unregistered event, and no error arrives.

Check two conditions:

- the widget is open in the placement where the event is registered. The current placement is returned by [BX24.placement.info](bx24-placement-info.md)
- the event name is passed without typos and with the correct capitalization. The list of available events is returned by [BX24.placement.getInterface](bx24-placement-get-interface.md)

## Continue Learning

- [{#T}](bx24-placement-info.md)
- [{#T}](bx24-placement-get-interface.md)
- [{#T}](bx24-placement-call.md)
- [{#T}](index.md)
