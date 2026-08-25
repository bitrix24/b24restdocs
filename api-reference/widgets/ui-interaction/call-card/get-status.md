# Get Call Status getStatus

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — access to the call card placement
>
> Who can execute the command: any user

The `getStatus` command returns the current data of the call card.

{% note info "" %}

The command operates within the application context in the `CALL_CARD` placement. This is a JS interface command, not a REST method: it cannot be invoked with a request to `/rest/`.

{% endnote %}

## How to Call the Command

The command is invoked from the widget with the [BX24.placement.call](../bx24-placement-call.md) method. The third argument is the callback function, which receives the result of the command.

```js
BX24.placement.call('getStatus', {}, function (result) {
    console.log(result);
});
```

## Command Parameters

The command takes no parameters. Pass an empty object `{}` as the second argument.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.call('getStatus', {}, function (result) {
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

    // the shape of the result is described below on this page
    type CallStatus = {
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

    const status = await $b24.placement.call('getStatus') as CallStatus

    console.log(status.CALL_ID, status.CALL_STATE)
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


- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.placement.call(
            placement='getStatus',
            params={},
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

## Command Result

```json
{
    "CALL_ID": "E45D40253D1C2D2F.1774588815.822533",
    "PHONE_NUMBER": "+19999999666",
    "LINE_NUMBER": "reg151083",
    "LINE_NAME": "",
    "CRM_ENTITY_TYPE": "CONTACT",
    "CRM_ENTITY_ID": 797,
    "CRM_ACTIVITY_ID": 12043,
    "CRM_BINDINGS": [
        {
        "ENTITY_TYPE": "DEAL",
        "ENTITY_ID": 4615
        },
        {
        "ENTITY_TYPE": "COMPANY",
        "ENTITY_ID": 643
        }
    ],
    "CALL_DIRECTION": "outgoing",
    "CALL_STATE": "idle",
    "CALL_LIST_MODE": false
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **CALL_ID**
[`string`](../../../data-types.md) | The identifier of the call ||
|| **PHONE_NUMBER**
[`string`](../../../data-types.md) | The client's number.

Possible states:

- the client's number — the usual case
- `hidden` — the client hid their number
- the key does not arrive at all — the number is not identified ||
|| **LINE_NUMBER**
[`string`](../../../data-types.md) | The line number ||
|| **LINE_NAME**
[`string`](../../../data-types.md) | The name of the company's phone line.

It can be an empty string if the line name is not set ||
|| **CRM_ENTITY_TYPE**
[`string`](../../../data-types.md) | The symbolic code of the CRM item type the call is bound to: `LEAD`, `DEAL`, `CONTACT`, or `COMPANY`.

An empty string if the call is not bound to the CRM ||
|| **CRM_ENTITY_ID**
[`integer`](../../../data-types.md) | The identifier of the CRM item the call is bound to.

`0` if the call is not bound to the CRM. The full list of linked items arrives in `CRM_BINDINGS` ||
|| **CRM_ACTIVITY_ID**
[`integer`](../../../data-types.md) | The identifier of the CRM activity created for the call.

If there is no activity, the key either does not arrive at all or arrives as an empty string ||
|| **CRM_BINDINGS**
[`object[]`](../../../data-types.md) | The bindings of the call to CRM objects [(detailed description)](#crm_bindings) ||
|| **CALL_DIRECTION**
[`string`](../../../data-types.md) | The direction of the call.

Possible values:

- `incoming` — incoming call
- `outgoing` — outgoing call
- `callback` — callback ||
|| **CALL_STATE**
[`string`](../../../data-types.md) | The state of the call.

Possible values:

- `idle` — no connection
- `connecting` — establishing connection
- `connected` — connection established ||
|| **CALL_LIST_MODE**
[`boolean`](../../../data-types.md) | Indicator of the call campaign mode ||
|#

### CRM_BINDINGS Parameter {#crm_bindings}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE**
[`string`](../../../data-types.md) | The type of the CRM object: `LEAD`, `DEAL`, `CONTACT`, or `COMPANY` ||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | The identifier of the CRM object ||
|#

## Errors

The `getStatus` command has no error codes of its own: it either runs or is not invoked at all.

- If the widget is open outside the `CALL_CARD` placement, the placement interface ignores the unknown command and the callback function is not invoked
- Check the command name with the correct capitalization: the list of commands available in the current placement is returned by [BX24.placement.getInterface](../bx24-placement-get-interface.md)

## Continue Learning

- [{#T}](./disable-auto-close.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-entity-changed.md)
- [{#T}](./call-card-before-close.md)
- [{#T}](./call-card-call-state-changed.md)
- [{#T}](./index.md)
- [{#T}](../../telephony/call-card.md)
