# When Loading or Changing a CRM Object BackgroundCallCard::entityChanged

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`](../../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can subscribe: any user

The `BackgroundCallCard::entityChanged` event occurs when the card loads or changes the CRM object linked to the call.

The event arrives in three cases:

- the call card opened — in this case, the event arrives earlier than [BackgroundCallCard::initialized](./initialized.md)
- the call was bound to another CRM object, for example, the operator created a new lead or contact from the card
- Bitrix24 updated the CRM data for the call and reloaded the card — in this case, the event arrives with the same CRM object

That is why the event does not always mean that the binding has changed: compare `CRM_ENTITY_TYPE` and `CRM_ENTITY_ID` with the previous values.

If the call is not bound to the CRM, the event still arrives: `CRM_ENTITY_TYPE` is an empty string, and `CRM_ENTITY_ID` is `0`.

{% note info "" %}

The event operates within the application context in the `PAGE_BACKGROUND_WORKER` placement. This is a JS interface event, not a REST event: you cannot subscribe to it with a request to `/rest/`.

{% endnote %}

## What the Handler Receives

Data is passed to the callback `BX24.placement.bindEvent` {.b24-info}

```js
callback({
    "PHONE_NUMBER": "+19001234567",
    "CRM_ENTITY_TYPE": "CONTACT",
    "CRM_ENTITY_ID": 123
});
```

## Event Handler Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **PHONE_NUMBER**
[`string`](../../../../data-types.md) | The client's phone number ||
|| **CRM_ENTITY_TYPE**
[`string`](../../../../data-types.md) | The type of the CRM object linked to the call, for example, `CONTACT`.

An empty string if the call is not bound to the CRM ||
|| **CRM_ENTITY_ID**
[`integer`](../../../../data-types.md) | The identifier of the CRM object linked to the call.

`0` if the call is not bound to the CRM ||
|#

## Event Subscription Parameters

The handler is registered from the widget with the [BX24.placement.bindEvent](../../bx24-placement-bind-event.md) method.

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../../../data-types.md) | The name of the interface event.

For this event — `BackgroundCallCard::entityChanged` ||
|| **callback***
[`callable`](../../../../data-types.md) | The function Bitrix24 invokes when the event occurs. The handler arguments are described above ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.bindEvent('BackgroundCallCard::entityChanged', function (eventData) {
                console.log(eventData);
            });
        });
    });
    ```

- JS (TS)

    ```ts
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide)
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    await $b24.placement.bindEvent('BackgroundCallCard::entityChanged', (eventData: { PHONE_NUMBER: string; CRM_ENTITY_TYPE: string; CRM_ENTITY_ID: number }) => {
      console.log(eventData.CRM_ENTITY_ID)
    })
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        await $b24.placement.bindEvent('BackgroundCallCard::entityChanged', (eventData) => {
          console.log(eventData)
        })
      })
    </script>
    ```

{% endlist %}

## Errors

Check the following conditions.

- The widget is open in the `PAGE_BACKGROUND_WORKER` placement. In other placements, the `BackgroundCallCard::*` events are not registered, and the subscription silently fails
- The event name is passed without typos and with the correct capitalization. The list of events available in the current placement is returned by [BX24.placement.getInterface](../../bx24-placement-get-interface.md)
- The call was raised by the application with the [telephony.externalCall.register](../../../../telephony/telephony-external-call-register.md) method. For calls made by Bitrix24 itself, the `BackgroundCallCard::*` events are not emitted at all

## Continue Learning

- [{#T}](../../bx24-placement-bind-event.md)
- [{#T}](./initialized.md)
- [{#T}](./index.md)
- [{#T}](../card.md)
- [{#T}](../index.md)
