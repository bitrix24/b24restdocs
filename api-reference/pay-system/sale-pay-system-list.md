# Get a List of Payment Systems sale.paysystem.list

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to change settings")

This method returns a list of payment systems.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **SELECT**
[`array`](../data-types.md) | An array containing the list of fields to select (see fields of the object [`sale_paysystem`](../sale/data-types.md)).
 
If the array is not provided or an empty array is passed, all available fields of the payment systems will be selected.
||
|| **FILTER**
[`object`](../data-types.md) | An object for filtering the selected payment systems in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [sale_paysystem](../sale/data-types.md). 

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` character should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` character should not be included in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` character should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **ORDER**
[`object`](../data-types.md) | An object for sorting the selected payment systems in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [sale_paysystem](../sale/data-types.md).

Possible values for `order`:
- `ASC` — in ascending order
- `DESC` — in descending order
||
|#

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PERSON_TYPE_ID","NAME","PSA_NAME","SORT","DESCRIPTION","ACTION_FILE","RESULT_FILE","NEW_WINDOW","TARIF","PS_MODE","HAVE_PAYMENT","HAVE_ACTION","HAVE_RESULT","HAVE_PREPAY","HAVE_PRICE","HAVE_RESULT_RECEIVE","ENCODING","ACTIVE","ALLOW_EDIT_PAYMENT","IS_CASH","AUTO_CHANGE_1C","CAN_PRINT_CHECK","ENTITY_REGISTRY_TYPE","XML_ID"],"FILTER":{"@ID":[117,118]},"ORDER":{"SORT":"ASC","ID":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PERSON_TYPE_ID","NAME","PSA_NAME","SORT","DESCRIPTION","ACTION_FILE","RESULT_FILE","NEW_WINDOW","TARIF","PS_MODE","HAVE_PAYMENT","HAVE_ACTION","HAVE_RESULT","HAVE_PREPAY","HAVE_PRICE","HAVE_RESULT_RECEIVE","ENCODING","ACTIVE","ALLOW_EDIT_PAYMENT","IS_CASH","AUTO_CHANGE_1C","CAN_PRINT_CHECK","ENTITY_REGISTRY_TYPE","XML_ID"],"FILTER":{"@ID":[117,118]},"ORDER":{"SORT":"ASC","ID":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.list
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.paysystem.list', {
            SELECT: [
                "ID",
                "PERSON_TYPE_ID",
                "NAME",
                "PSA_NAME",
                "SORT",
                "DESCRIPTION",
                "ACTION_FILE",
                "RESULT_FILE",
                "NEW_WINDOW",
                "TARIF",
                "PS_MODE",
                "HAVE_PAYMENT",
                "HAVE_ACTION",
                "HAVE_RESULT",
                "HAVE_PREPAY",
                "HAVE_PRICE",
                "HAVE_RESULT_RECEIVE",
                "ENCODING",
                "ACTIVE",
                "ALLOW_EDIT_PAYMENT",
                "IS_CASH",
                "AUTO_CHANGE_1C",
                "CAN_PRINT_CHECK",
                "ENTITY_REGISTRY_TYPE",
                "XML_ID",
            ],
            FILTER: {
                "@ID": [117, 118],
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
        'sale.paysystem.list',
        [
            'SELECT' => [
                "ID",
                "PERSON_TYPE_ID",
                "NAME",
                "PSA_NAME",
                "SORT",
                "DESCRIPTION",
                "ACTION_FILE",
                "RESULT_FILE",
                "NEW_WINDOW",
                "TARIF",
                "PS_MODE",
                "HAVE_PAYMENT",
                "HAVE_ACTION",
                "HAVE_RESULT",
                "HAVE_PREPAY",
                "HAVE_PRICE",
                "HAVE_RESULT_RECEIVE",
                "ENCODING",
                "ACTIVE",
                "ALLOW_EDIT_PAYMENT",
                "IS_CASH",
                "AUTO_CHANGE_1C",
                "CAN_PRINT_CHECK",
                "ENTITY_REGISTRY_TYPE",
                "XML_ID",
            ],
            'FILTER' => [
                "@ID" => [117, 118],
            ],
            'ORDER' => [
                "SORT" => "ASC",
                "ID" => "DESC",
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result":[
        {
            "NAME":"New Card Payment",
            "PSA_NAME":"New Card Payment",
            "DESCRIPTION":"Easily pay for purchases with a card.",
            "ACTION_FILE":"resthandlerform",
            "RESULT_FILE":null,
            "NEW_WINDOW":"N",
            "PS_MODE":null,
            "HAVE_PAYMENT":"N",
            "HAVE_ACTION":"N",
            "HAVE_RESULT":"N",
            "HAVE_PREPAY":"N",
            "HAVE_PRICE":"N",
            "HAVE_RESULT_RECEIVE":"Y",
            "ENCODING":null,
            "ACTIVE":"Y",
            "ALLOW_EDIT_PAYMENT":"Y",
            "IS_CASH":"N",
            "AUTO_CHANGE_1C":"N",
            "CAN_PRINT_CHECK":"N",
            "ENTITY_REGISTRY_TYPE":"ORDER",
            "XML_ID":"my_ps_id",
            "ID":118,
            "PERSON_TYPE_ID":3,
            "SORT":100,
            "TARIFF":null
        },
        {
            "NAME":"Card Payment",
            "PSA_NAME":"Card Payment",
            "DESCRIPTION":"Easily pay for purchases with a card.",
            "ACTION_FILE":"resthandlerform",
            "RESULT_FILE":null,
            "NEW_WINDOW":"Y",
            "PS_MODE":"",
            "HAVE_PAYMENT":"N",
            "HAVE_ACTION":"N",
            "HAVE_RESULT":"N",
            "HAVE_PREPAY":"N",
            "HAVE_PRICE":"N",
            "HAVE_RESULT_RECEIVE":"Y",
            "ENCODING":"windows-1251",
            "ACTIVE":"Y",
            "ALLOW_EDIT_PAYMENT":"Y",
            "IS_CASH":"N",
            "AUTO_CHANGE_1C":"N",
            "CAN_PRINT_CHECK":"N",
            "ENTITY_REGISTRY_TYPE":"ORDER",
            "XML_ID":"my_ps_id",
            "ID":117,
            "PERSON_TYPE_ID":3,
            "SORT":100,
            "TARIFF":null
        }
    ],
    "time":{
        "start":1714993577.419989,
        "finish":1714993577.621777,
        "duration":0.20178794860839844,
        "processing":0.027898073196411133,
        "date_start":"2024-05-06T14:06:17+03:00",
        "date_finish":"2024-05-06T14:06:17+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_paysystem[]`](../sale/data-types.md) | An array of objects containing information about the selected payment systems ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error":"ACCESS_DENIED",
    "error_description":"Access denied!"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | 400 ||
|| `ACCESS_DENIED` | Insufficient permissions to retrieve the list of payment systems | 403 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-pay-system-handler-add.md)
- [{#T}](./sale-pay-system-handler-update.md)
- [{#T}](./sale-pay-system-handler-list.md)
- [{#T}](./sale-pay-system-handler-delete.md)
- [{#T}](./sale-pay-system-add.md)
- [{#T}](./sale-pay-system-update.md)
- [{#T}](./sale-pay-system-settings-get.md)
- [{#T}](./sale-pay-system-settings-update.md)
- [{#T}](./sale-pay-system-delete.md)
- [{#T}](./sale-pay-system-pay-payment.md)
- [{#T}](./sale-pay-system-pay-invoice.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)
- [{#T}](./sale-pay-system-settings-invoice-get.md)