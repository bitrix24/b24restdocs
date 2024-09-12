# Get a List of Bindings sale.paymentItemShipment.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of payment bindings to shipments based on a filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to be selected (see fields of the [sale_payment_item_shipment](../data-types.md#sale_payment_item_shipment) object).
 
If not provided or an empty array is passed, all available fields of payment bindings to shipments will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected payment bindings to shipments in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_payment_item_shipment](../data-types.md) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:

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
[`object`](../../data-types.md) | An object for sorting the selected payment bindings to shipments in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_payment_item_shipment](../data-types.md) object.
 
Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, the value `50` must be passed. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` is the desired page number
 ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","shipmentId","paymentId","xmlId","dateInsert"],"filter":{"paymentId":1025,"shipmentId":2467},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitemshipment.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","shipmentId","paymentId","xmlId","dateInsert"],"filter":{"paymentId":1025,"shipmentId":2467},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitemshipment.list
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.paymentitemshipment.list", {
            "select": [
                "id",
                "shipmentId",
                "paymentId",
                "xmlId",
                "dateInsert",
            ],
            "filter": {
                "paymentId": 1025,
                "shipmentId": 2467,
            },
            "order": {
                "id": "desc",
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paymentitemshipment.list',
        [
            'select' => [
                'id',
                'shipmentId',
                'paymentId',
                'xmlId',
                'dateInsert',
            ],
            'filter' => [
                'paymentId' => 1025,
                'shipmentId' => 2467,
            ],
            'order' => [
                'id' => 'desc',
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
    "result":{
        "paymentItemsShipment":[
            {
                "dateInsert":"2024-04-11T16:59:21+03:00",
                "id":1180,
                "paymentId":1025,
                "shipmentId":2467,
                "xmlId":"bx_6617fac9afe1e"
            }
        ]
    },
    "total":1,
    "time":{
        "start":1713172802.193215,
        "finish":1713172802.464073,
        "duration":0.2708580493927002,
        "processing":0.018366098403930664,
        "date_start":"2024-04-15T12:20:02+03:00",
        "date_finish":"2024-04-15T12:20:02+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **paymentItemsShipment**
[`sale_payment_item_shipment[]`](../data-types.md) | An array of objects containing information about the selected payment bindings to shipments ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read the elements of the shipment's tabular part ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-shipment-add.md)
- [{#T}](./sale-payment-item-shipment-update.md)
- [{#T}](./sale-payment-item-shipment-get.md)
- [{#T}](./sale-payment-item-shipment-delete.md)
- [{#T}](./sale-payment-item-shipment-get-fields.md)