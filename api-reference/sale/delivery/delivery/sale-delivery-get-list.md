# Get Delivery Services List sale.delivery.getlist

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

This method retrieves a list of delivery services.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SELECT**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the object [`sale_delivery_service`](../../data-types.md)).
 
If not provided or an empty array is passed, all available fields of delivery services will be selected.
||
|| **FILTER**
[`object`](../../data-types.md) | An object for filtering the selected delivery services in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.
 
Possible values for `field` correspond to the fields of the object [`sale_delivery_service`](../../data-types.md).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:

- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string
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
|| **ORDER**
[`object`](../../data-types.md) | An object for sorting the selected delivery services in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.
 
Possible values for `field` correspond to the fields of the object [`sale_delivery_service`](../../data-types.md).
 
Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PARENT_ID","NAME","ACTIVE","DESCRIPTION","SORT","CURRENCY"],"FILTER":{"@ID":[196,197,198]},"ORDER":{"SORT":"ASC","ID":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.getlist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PARENT_ID","NAME","ACTIVE","DESCRIPTION","SORT","CURRENCY"],"FILTER":{"@ID":[196,197,198]},"ORDER":{"SORT":"ASC","ID":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.getlist
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.getlist', {
            SELECT: [
                "ID",
                "PARENT_ID",
                "NAME",
                "ACTIVE",
                "DESCRIPTION",
                "SORT",
                "CURRENCY",
            ],
            FILTER: {
                "@ID": [196, 197, 198],
            },
            ORDER: {

                SORT: "ASC",
                ID: "DESC",
            },
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
        'sale.delivery.getlist',
        [
            'SELECT' => [
                "ID",
                "PARENT_ID",
                "NAME",
                "ACTIVE",
                "DESCRIPTION",
                "SORT",
                "CURRENCY",
            ],
            'FILTER' => [
                "@ID" => [196, 197, 198],
            ],
            'ORDER' => [
                "SORT" => "ASC",
                "ID" => "DESC",
            ]
        ]
    );
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result":[
        {
            "NAME":"Cargo",
            "ACTIVE":"Y",
            "DESCRIPTION":"Cargo Delivery",
            "CURRENCY":"USD",
            "ID":198,
            "PARENT_ID":196,
            "SORT":500
        },
        {
            "NAME":"Taxi",
            "ACTIVE":"Y",
            "DESCRIPTION":"Taxi Delivery",
            "CURRENCY":"USD",
            "ID":197,
            "PARENT_ID":196,
            "SORT":500
        },
        {
            "NAME":"Uber Taxi",
            "ACTIVE":"Y",
            "DESCRIPTION":"Uber Taxi Description",
            "CURRENCY":"USD",
            "ID":196,
            "PARENT_ID":null,
            "SORT":500
        }
    ],
    "time":{
        "start":1714141661.751638,
        "finish":1714141661.945714,
        "duration":0.1940760612487793,
        "processing":0.013057947158813477,
        "date_start":"2024-04-26T17:27:41+03:00",
        "date_finish":"2024-04-26T17:27:41+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_delivery_service[]`](../../data-types.md) | An array of objects containing information about the selected delivery services ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
   "error":"ACCESS_DENIED",
   "error_description":"Access denied!"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | 400 ||
|| `ACCESS_DENIED` | Insufficient permissions to retrieve the list of delivery services | 403 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-add.md)
- [{#T}](./sale-delivery-delete.md)
- [{#T}](./sale-delivery-update.md)
- [{#T}](./sale-delivery-config-update.md)
- [{#T}](./sale-delivery-config-get.md)