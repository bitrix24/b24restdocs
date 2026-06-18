# Remove Order Binding to CRM Object crm.orderentity.deleteByFilter

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: online store administrator

This method removes the binding of an order to a CRM object.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values for removing the binding ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **orderId***
[`sale_order.id`](../../../sale/data-types.md#sale_order) | Order identifier ||
|| **ownerTypeId***
[`integer`](../../../data-types.md) | Identifier of the [CRM object type](../../data-types.md#object_type).

Binding is only possible to a deal or invoice
||
|| **ownerId***
[`integer`](../../../data-types.md) | Identifier of the CRM object.

For deals, it can be obtained using the [crm.deal.list](../../deals/crm-deal-list.md) method.

For invoices, it can be obtained using the [crm.invoice.list](../../outdated/invoice/crm-invoice-list.md)
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Add order binding to a deal:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5125,"ownerId":6933,"ownerTypeId":2}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.orderentity.deletebyfilter
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5125,"ownerId":6933,"ownerTypeId":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.orderentity.deletebyfilter
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'crm.orderentity.deletebyfilter',
        params: {
          fields: {
            orderId: 5125,
            ownerId: 6933,
            ownerTypeId: 2,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Order entity deleted:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function deleteOrderEntityByFilter() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.orderentity.deletebyfilter',
            params: {
              fields: {
                orderId: 5125,
                ownerId: 6933,
                ownerTypeId: 2,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Order entity deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteOrderEntityByFilter)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.orderentity.deletebyfilter',
                [
                    'fields' => [
                        'orderId'     => 5125,
                        'ownerId'     => 6933,
                        'ownerTypeId' => 2,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting order entity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.orderentity.deletebyfilter",
        {
            fields: {
                orderId: 5125,
                ownerId: 6933,
                ownerTypeId: 2
            }
        },
    )
        .then(
            function(result)
            {
                if (result.error())
                {
                    console.error(result.error());
                }
                else
                {
                    console.log(result);
                }
            },
            function(error)
            {
                console.info(error);
            }
        );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.orderentity.deletebyfilter',
        [
            'fields' => [
                'orderId' => 5125,
                'ownerId' => 6933,
                'ownerTypeId' => 2
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1719325693.109545,
        "finish": 1719325695.863527,
        "duration": 2.7539820671081543,
        "processing": 1.773292064666748,
        "date_start": "2024-06-25T16:28:13+02:00",
        "date_finish": "2024-06-25T16:28:15+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Indicates whether the operation was successful ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: ownerId, orderId"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Description** ||
|| `200040300020` | `Access Denied` 
Insufficient access permissions
||
|| `201640400004` | `entity relation is not exists` 
Order binding to the CRM object not found
||
|| `200540400001` | `order does not exist` 
Order not found
||
|| `0` | `Required fields: #FIELDS#` 
Required fields not specified (`#FIELDS#` — list of fields separated by commas)
||
|| `0` | Various order saving errors
||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-order-entity-add.md)
- [{#T}](./crm-order-entity-list.md)
- [{#T}](./crm-order-entity-get-fields.md)