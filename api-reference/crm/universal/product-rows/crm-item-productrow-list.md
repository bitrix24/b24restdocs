# Get product rows of the CRM object crm.item.productrow.list

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

The key can have an additional prefix that clarifies the behavior of the filter. Possible prefix values:

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

- JS

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

- PHP

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