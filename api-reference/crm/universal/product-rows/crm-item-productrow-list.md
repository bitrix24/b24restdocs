# Get product rows of the CRM object crm.item.productrow.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires read access permission for the CRM object whose product rows are being selected.

The method retrieves product rows of the CRM object.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../../data-types.md) | Object for filtering selected records in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [`crm_item_product_row`](../../data-types.md#crm_item_product_row) object.

**The following keys must be present:**

**=ownerType**
**=ownerId**

In `=ownerType`, pass the [Short symbolic code of the type](../../data-types.md#object_type).

The key may have an additional prefix that specifies the behavior of the filter. Possible prefix values:

- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol in the filter value does not need to be passed. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol needs to be passed in the value. Examples:
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol needs to be passed in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
||
|| **order**
[`object`](../../../data-types.md) | Object for sorting selected elements of the shipment table in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [`crm_item_product_row`](../../data-types.md#crm_item_product_row) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|| **start**
[`integer`](../../../data-types.md) | Parameter used for managing pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.

The formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` — the number of the desired page
 ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"=ownerType":"D","=ownerId":13142,">price":5000},"order":{"price":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.productrow.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"=ownerType":"D","=ownerId":13142,">price":5000},"order":{"price":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.productrow.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductRowListResult = {
      productRows: {
        id: number
        ownerId: number
        ownerType: string
        productId: number
        productName: string
        price: number
        priceAccount: number
        priceExclusive: number
        priceNetto: number
        priceBrutto: number
        quantity: number
        discountTypeId: number
        discountRate: number
        discountSum: number
        taxRate: number | null
        taxIncluded: string
        customized: string
        measureCode: number
        measureName: string
        sort: number
        xmlId: string
        type: number
        storeId: number
      }[]
    }

    try {
      // crm.item.productrow.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ProductRowListResult>({
        method: 'crm.item.productrow.list',
        params: {
          filter: {
            '=ownerType': 'D',
            '=ownerId': 13142,
            '>price': 5000,
          },
          order: {
            price: 'desc',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.productRows.length, result.productRows)
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
      async function listProductRows() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.item.productrow.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.productrow.list',
            params: {
              filter: {
                '=ownerType': 'D',
                '=ownerId': 13142,
                '>price': 5000,
              },
              order: {
                price: 'desc',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.productRows.length, result.productRows)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listProductRows)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.productrow.list',
                [
                    'filter' => [
                        "=ownerType" => 'D',
                        "=ownerId"   => 13142,
                        ">price"     => 5000,
                    ],
                    'order'  => [
                        'price' => "desc"
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.productrow.list', {
            filter: {
                "=ownerType": 'D',
                "=ownerId": 13142,
                ">price": 5000,
            },
            order: {
                price: "desc"
            },
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.productrow.list',
        [
            'filter' => [
                "=ownerType" => 'D',
                "=ownerId" => 13142,
                ">price" => 5000,
            ],
            'order' => [
                'price' => "desc"
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
   "result": {
      "productRows": [
         {
            "id": 17649,
            "ownerId": 13142,
            "ownerType": "D",
            "productId": 9621,
            "productName": "iphone 14",
            "price": 90000,
            "priceAccount": 90000,
            "priceExclusive": 81818.18,
            "priceNetto": 90909.09,
            "priceBrutto": 100000,
            "quantity": 3,
            "discountTypeId": 2,
            "discountRate": 10,
            "discountSum": 9090.91,
            "taxRate": 10,
            "taxIncluded": "Y",
            "customized": "Y",
            "measureCode": 796,
            "measureName": "pcs",
            "sort": 20,
            "xmlId": "sale_basket_8147",
            "type": 4,
            "storeId": 19
         },
         {
            "id": 17650,
            "ownerId": 13142,
            "ownerType": "D",
            "productId": 9623,
            "productName": "iphone 10xs",
            "price": 5550,
            "priceAccount": 5550,
            "priceExclusive": 5550,
            "priceNetto": 5550,
            "priceBrutto": 5550,
            "quantity": 1,
            "discountTypeId": 2,
            "discountRate": 0,
            "discountSum": 0,
            "taxRate": null,
            "taxIncluded": "Y",
            "customized": "Y",
            "measureCode": 6,
            "measureName": "m",
            "sort": 10,
            "xmlId": "sale_basket_8148",
            "type": 4,
            "storeId": 17
         }
      ]
   },
   "total": 2,
   "time": {
      "start": 1716905609.186602,
      "finish": 1716905609.434087,
      "duration": 0.24748492240905762,
      "processing": 0.06894516944885254,
      "date_start": "2024-05-28T17:13:29+02:00",
      "date_finish": "2024-05-28T17:13:29+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **productRows**
[`crm_item_product_row[]`](../../data-types.md#crm_item_product_row) | Array of objects containing information about the selected product rows of the CRM object ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error": "ACCESS_DENIED",
   "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Access denied ||
|| `INVALID_ARG_VALUE` | Invalid values for input parameters ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-productrow-add.md)
- [{#T}](./crm-item-productrow-fields.md)
- [{#T}](./crm-item-productrow-get.md)
- [{#T}](./crm-item-productrow-set.md)
- [{#T}](./crm-item-productrow-update.md)
- [{#T}](./crm-item-productrow-get-available-for-payment.md)
- [{#T}](./crm-item-productrow-delete.md)

