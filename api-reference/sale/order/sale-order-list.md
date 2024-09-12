# Get the list of orders sale.order.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.order.list` retrieves a list of orders.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the object [sale_order](../data-types.md#sale_order)).

If not provided or an empty array is passed, all available order fields will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected orders in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.
 
Possible values for `field` correspond to the fields of the object [sale_order](../data-types.md#sale_order).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:

- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - "mol%" — searching for values starting with "mol"
    - "%mol" — searching for values ending with "mol"
    - "%mol%" — searching for values where "mol" can be in any position

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.

- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - "mol%" — searching for values not starting with "mol"
    - "%mol" — searching for values not ending with "mol"
    - "%mol%" — searching for values where the substring "mol" is not in any position

- `!%=` — NOT LIKE (see description above)

- `=` — equal, exact match (used by default)
- `!=` - not equal
- `!` — not equal ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected orders in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.
 
Possible values for `field` correspond to the fields of the object [sale_order](../data-types.md#sale_order).
 
Possible values for `order`:

- asc — ascending order
- desc — descending order
 ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for pagination control.
 
The page size of results is always static: 50 records.
 
To select the second page of results, the value `50` must be passed. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` is the desired page number
 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","lid","dateInsert","dateUpdate","personTypeId","personTypeXmlId","statusId","dateStatus","empStatusId","marked","dateMarked","empMarkedId","reasonMarked","price","discountValue","taxValue","userDescription","additionalInfo","comments","companyId","responsibleId","recurringId","lockedBy","dateLock","recountFlag","affiliateId","updated1c","orderTopic","xmlId","statusXmlId","id1c","version","version1c","externalOrder","canceled","dateCanceled","empCanceledId","reasonCanceled","userId","currency","accountNumber","payed","deducted"],"filter":{"<id":10,"@personTypeId":[3,4],"payed":"N"},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.order.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","lid","dateInsert","dateUpdate","personTypeId","personTypeXmlId","statusId","dateStatus","empStatusId","marked","dateMarked","empMarkedId","reasonMarked","price","discountValue","taxValue","userDescription","additionalInfo","comments","companyId","responsibleId","recurringId","lockedBy","dateLock","recountFlag","affiliateId","updated1c","orderTopic","xmlId","statusXmlId","id1c","version","version1c","externalOrder","canceled","dateCanceled","empCanceledId","reasonCanceled","userId","currency","accountNumber","payed","deducted"],"filter":{"<id":10,"@personTypeId":[3,4],"payed":"N"},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.order.list
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.order.list", {
            "select": [
                "id",
                "lid",
                "dateInsert",
                "dateUpdate",
                "personTypeId",
                "personTypeXmlId",
                "statusId",
                "dateStatus",
                "empStatusId",
                "marked",
                "dateMarked",
                "empMarkedId",
                "reasonMarked",
                "price",
                "discountValue",
                "taxValue",
                "userDescription",
                "additionalInfo",
                "comments",
                "companyId",
                "responsibleId",
                "recurringId",
                "lockedBy",
                "dateLock",
                "recountFlag",
                "affiliateId",
                "updated1c",
                "orderTopic",
                "xmlId",
                "statusXmlId",
                "id1c",
                "version",
                "version1c",
                "externalOrder",
                "canceled",
                "dateCanceled",
                "empCanceledId",
                "reasonCanceled",
                "userId",
                "currency",
                "accountNumber",
                "payed",
                "deducted",
            ],
            "filter": {
                "<id": 10,
                "@personTypeId": [3, 4],
                "payed": "N",
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
        'sale.order.list',
        [
            'select' => [
                "id",
                "lid",
                "dateInsert",
                "dateUpdate",
                "personTypeId",
                "personTypeXmlId",
                "statusId",
                "dateStatus",
                "empStatusId",
                "marked",
                "dateMarked",
                "empMarkedId",
                "reasonMarked",
                "price",
                "discountValue",
                "taxValue",
                "userDescription",
                "additionalInfo",
                "comments",
                "companyId",
                "responsibleId",
                "recurringId",
                "lockedBy",
                "dateLock",
                "recountFlag",
                "affiliateId",
                "updated1c",
                "orderTopic",
                "xmlId",
                "statusXmlId",
                "id1c",
                "version",
                "version1c",
                "externalOrder",
                "canceled",
                "dateCanceled",
                "empCanceledId",
                "reasonCanceled",
                "userId",
                "currency",
                "accountNumber",
                "payed",
                "deducted",
            ],
            'filter' => [
                "<id" => 10,
                "@personTypeId" => [3, 4],
                "payed" => "N",
            ],
            'order' => [
                "id" => "desc",
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
    "result": {
        "orders": [
            {
                "accountNumber": "165",
                "additionalInfo": "",
                "affiliateId": null,
                "canceled": "N",
                "comments": "",
                "companyId": null,
                "currency": "USD",
                "dateCanceled": null,
                "dateInsert": "2022-10-14T17:19:11+03:00",
                "dateLock": null,
                "dateMarked": null,
                "dateStatus": "2022-10-14T17:19:03+03:00",
                "dateUpdate": "2022-10-14T17:19:11+03:00",
                "deducted": "N",
                "discountValue": 0,
                "empCanceledId": null,
                "empMarkedId": null,
                "empStatusId": 1,
                "externalOrder": "N",
                "id": 9,
                "id1c": "",
                "lid": "s1",
                "lockedBy": "",
                "marked": "N",
                "orderTopic": "",
                "payed": "N",
                "personTypeId": 4,
                "personTypeXmlId": "",
                "price": 1176,
                "reasonCanceled": "",
                "reasonMarked": "",
                "recountFlag": "Y",
                "recurringId": "",
                "responsibleId": 1,
                "statusId": "N",
                "statusXmlId": "",
                "taxValue": 196,
                "updated1c": "N",
                "userDescription": "",
                "userId": 2,
                "version": 0,
                "version1c": "",
                "xmlId": "bx_63498bf7c8d31"
            },
        ]
    },
    "total": 1,
    "time": {
        "start": 1712847891.436862,
        "finish": 1712847892.028163,
        "duration": 0.5913009643554688,
        "processing": 0.1332709789276123,
        "date_start": "2024-04-11T18:04:51+03:00",
        "date_finish": "2024-04-11T18:04:52+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **orders**
[`sale_order[]`](../data-types.md) | An array of objects containing information about the selected orders ||
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
|| `200040300010` | Insufficient permissions to read orders ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-order-add.md)
- [{#T}](./sale-order-update.md)
- [{#T}](./sale-order-get.md)
- [{#T}](./sale-order-delete.md)
- [{#T}](./sale-order-get-fields.md)