# Client Change Event CallCard::EntityChanged

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — access to the call card placement
>
> Who can subscribe: any user

The `CallCard::EntityChanged` event occurs when the client bound to the call changes.

The event arrives in four cases:

- the call card opened and pulled up CRM data
- the card refreshed the client data
- a CRM item was created or bound from the card — a lead, a contact, or a company
- in call campaign mode, the operator moved on to the next client

{% note info "" %}

The event operates within the application context in the `CALL_CARD` placement. This is a JS interface event, not a REST event: you cannot subscribe to it with a request to `/rest/`.

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

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **PHONE_NUMBER***
[`string`](../../../data-types.md) | The client's phone number.

If the client has no phone number at all, the string `unknown` arrives ||
|| **CRM_ENTITY_TYPE***
[`string`](../../../data-types.md) | The type of the CRM object linked to the call ||
|| **CRM_ENTITY_ID***
[`integer`](../../../data-types.md) | The identifier of the CRM object linked to the call ||
|#

## Event Subscription Parameters

The handler is registered from the widget with the [BX24.placement.bindEvent](../bx24-placement-bind-event.md) method.

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | The name of the interface event.

For this event — `CallCard::EntityChanged` ||
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
            BX24.placement.bindEvent('CallCard::EntityChanged', function (eventData) {
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

    await $b24.placement.bindEvent('CallCard::EntityChanged', (eventData: { PHONE_NUMBER: string; CRM_ENTITY_TYPE: string; CRM_ENTITY_ID: number }) => {
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

        await $b24.placement.bindEvent('CallCard::EntityChanged', (eventData) => {
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
            placement='CallCard::EntityChanged',
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

- The widget is open in the `CALL_CARD` placement. In other placements, the `CallCard::*` events are not registered, and the subscription silently fails
- The event name is passed without typos and with the correct capitalization. The list of events available in the current placement is returned by [BX24.placement.getInterface](../bx24-placement-get-interface.md)

## Continue Learning

- [{#T}](./get-status.md)
- [{#T}](./disable-auto-close.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-before-close.md)
- [{#T}](./call-card-call-state-changed.md)
- [{#T}](./index.md)
- [{#T}](../../telephony/call-card.md)
