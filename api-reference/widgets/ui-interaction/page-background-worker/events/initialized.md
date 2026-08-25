# After the Call Card Is Created BackgroundCallCard::initialized

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can subscribe: any user

The `BackgroundCallCard::initialized` event occurs after the call card is created and the initial data is passed.

{% note info "" %}

The event operates within the application context in the `PAGE_BACKGROUND_WORKER` placement. This is a JS interface event, not a REST event: you cannot subscribe to it with a request to `/rest/`.

{% endnote %}

## What the Handler Receives

Data is passed to the callback `BX24.placement.bindEvent` {.b24-info}

```js
callback({
    "CALL_ID": "E45D40253D1C2D2F.1774588815.822533",
    "PHONE_NUMBER": "+19001234567",
    "LINE_NUMBER": "reg151083",
    "LINE_NAME": "",
    "CRM_ENTITY_TYPE": "CONTACT",
    "CRM_ENTITY_ID": 123,
    "CRM_ACTIVITY_ID": 456,
    "CRM_BINDINGS": [{"ENTITY_TYPE": "DEAL", "ENTITY_ID": 789}],
    "CALL_DIRECTION": "outgoing",
    "CALL_STATE": "idle",
    "CALL_LIST_MODE": false
});
```

## Event Handler Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **CALL_ID**
[`string`](../../../../data-types.md) | The identifier of the call ||
|| **PHONE_NUMBER**
[`string`](../../../../data-types.md) | The client's number.

The key does not arrive at all if the number is not identified ||
|| **LINE_NUMBER**
[`string`](../../../../data-types.md) | The line number ||
|| **LINE_NAME**
[`string`](../../../../data-types.md) | The name of the company's phone line.

It can be an empty string if the line name is not set ||
|| **CRM_ENTITY_TYPE**
[`string`](../../../../data-types.md) | The type of the current CRM object: `LEAD`, `CONTACT`, `COMPANY`, or `DEAL`.

An empty string if the call is not bound to the CRM ||
|| **CRM_ENTITY_ID**
[`integer`](../../../../data-types.md) | The identifier of the CRM object the call is bound to.

`0` if the call is not bound to the CRM ||
|| **CRM_ACTIVITY_ID**
[`integer`](../../../../data-types.md) | The identifier of the CRM activity created for the call.

If there is no activity, the key either does not arrive at all or arrives as an empty string ||
|| **CRM_BINDINGS**
[`object[]`](../../../../data-types.md) | The bindings of the call to CRM objects [(detailed description)](#crm_bindings) ||
|| **CALL_DIRECTION**
[`string`](../../../../data-types.md) | The direction of the call.

Possible values:

- `incoming` — incoming call
- `outgoing` — outgoing call
- `callback` — callback ||
|| **CALL_STATE**
[`string`](../../../../data-types.md) | The state of the call.

Possible values:

- `idle` — no connection
- `connecting` — establishing connection
- `connected` — connection established ||
|| **CALL_LIST_MODE**
[`boolean`](../../../../data-types.md) | Indicator of the call campaign mode ||
|#

### CRM_BINDINGS Parameter {#crm_bindings}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE**
[`string`](../../../../data-types.md) | The type of the CRM object ||
|| **ENTITY_ID**
[`integer`](../../../../data-types.md) | The identifier of the CRM object ||
|#

## Event Subscription Parameters

The handler is registered from the widget with the [BX24.placement.bindEvent](../../bx24-placement-bind-event.md) method.

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../../../data-types.md) | The name of the interface event.

For this event — `BackgroundCallCard::initialized` ||
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
            BX24.placement.bindEvent('BackgroundCallCard::initialized', function (eventData) {
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

    type CallCardData = {
      CALL_ID: string
      PHONE_NUMBER?: string
      LINE_NUMBER: string
      LINE_NAME: string
      CRM_ENTITY_TYPE: string
      CRM_ENTITY_ID: number
      CRM_ACTIVITY_ID?: number | string
      CRM_BINDINGS: Array<{ ENTITY_TYPE: string; ENTITY_ID: number }>
      CALL_DIRECTION: string
      CALL_STATE: string
      CALL_LIST_MODE: boolean
    }

    await $b24.placement.bindEvent('BackgroundCallCard::initialized', (eventData: CallCardData) => {
      console.log(eventData.CALL_ID)
    })
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        await $b24.placement.bindEvent('BackgroundCallCard::initialized', (eventData) => {
          console.log(eventData)
        })
      })
    </script>
    ```


- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.placement.bind_event(
            placement='BackgroundCallCard::initialized',
            handler='**your_handler_url_here**',
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```
{% endlist %}

## Errors

Check the following conditions.

- The widget is open in the `PAGE_BACKGROUND_WORKER` placement. In other placements, the `BackgroundCallCard::*` events are not registered, and the subscription silently fails
- The event name is passed without typos and with the correct capitalization. The list of events available in the current placement is returned by [BX24.placement.getInterface](../../bx24-placement-get-interface.md)
- The call was raised by the application with the [telephony.externalCall.register](../../../../telephony/telephony-external-call-register.md) method. For calls made by Bitrix24 itself, the `BackgroundCallCard::*` events are not emitted at all

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../card.md)
- [{#T}](../index.md)
