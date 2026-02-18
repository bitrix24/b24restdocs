# Get a list of payment systems sale.paysystem.list

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to change settings")

The method returns a list of payment systems.

## Method parameters

#|
|| **Name**
`type` | **Description** ||
|| **SELECT**
[`array`](../data-types.md) | An array containing the list of fields to select (see fields of the [`sale_paysystem`](../sale/data-types.md) object).
 
If the array is not provided or an empty array is passed, all available fields of the payment systems will be selected.
||
|| **FILTER**
[`object`](../data-types.md) | An object for filtering the selected payment systems in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_paysystem](../sale/data-types.md) object. 

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
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

Possible values for `field` correspond to the fields of the [sale_paysystem](../sale/data-types.md) object.

Possible values for `order`:
- `ASC` — in ascending order
- `DESC` — in descending order
||
|#

## Code examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PERSON_TYPE_ID","NAME","PSA_NAME","SORT","DESCRIPTION","ACTION_FILE","RESULT_FILE","NEW_WINDOW","PLAN","PS_MODE","HAVE_PAYMENT","HAVE_ACTION","HAVE_RESULT","HAVE_PREPAY","HAVE_PRICE","HAVE_RESULT_RECEIVE","ENCODING","ACTIVE","ALLOW_EDIT_PAYMENT","IS_CASH","AUTO_CHANGE_QUICKBOOKS","CAN_PRINT_CHECK","ENTITY_REGISTRY_TYPE","XML_ID"],"FILTER":{"@ID":[117,118]},"ORDER":{"SORT":"ASC","ID":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PERSON_TYPE_ID","NAME","PSA_NAME","SORT","DESCRIPTION","ACTION_FILE","RESULT_FILE","NEW_WINDOW","PLAN","PS_MODE","HAVE_PAYMENT","HAVE_ACTION","HAVE_RESULT","HAVE_PREPAY","HAVE_PRICE","HAVE_RESULT_RECEIVE","ENCODING","ACTIVE","ALLOW_EDIT_PAYMENT","IS_CASH","AUTO_CHANGE_QUICKBOOKS","CAN_PRINT_CHECK","ENTITY_REGISTRY_TYPE","XML_ID"],"FILTER":{"@ID":[117,118]},"ORDER":{"SORT":"ASC","ID":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'sale.paysystem.list',
        {
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
            "PLAN",
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
            "AUTO_CHANGE_QUICKBOOKS",
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
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('sale.paysystem.list', {
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
          "PLAN",
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
          "AUTO_CHANGE_QUICKBOOKS",
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
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('sale.paysystem.list', {
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
          "PLAN",
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
          "AUTO_CHANGE_QUICKBOOKS",
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
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
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
                        "PLAN",
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
                        "AUTO_CHANGE_QUICKBOOKS",
                        "CAN_PRINT_CHECK",
                        "ENTITY_REGISTRY_TYPE",
                        "XML_ID",
                    ],
                    'FILTER' => [
                        "@ID" => [117, 118],
                    ],
                    'ORDER' => [
                        'SORT' => "ASC",
                        'ID'   => "DESC",
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching payment system list: ' . $e->getMessage();
    }
    ```

- BX24.js

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
                "PLAN",
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
                "AUTO_CHANGE_QUICKBOOKS",
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

- PHP CRest

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
                "PLAN",
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
                "AUTO_CHANGE_QUICKBOOKS",
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

## Response handling

HTTP status: **200**

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
            "AUTO_CHANGE_QUICKBOOKS":"N",
            "CAN_PRINT_CHECK":"N",
            "ENTITY_REGISTRY_TYPE":"ORDER",
            "XML_ID":"my_ps_id",
            "ID":118,
            "PERSON_TYPE_ID":3,
            "SORT":100,
            "PLAN":null
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
            "AUTO_CHANGE_QUICKBOOKS":"N",
            "CAN_PRINT_CHECK":"N",
            "ENTITY_REGISTRY_TYPE":"ORDER",
            "XML_ID":"my_ps_id",
            "ID":117,
            "PERSON_TYPE_ID":3,
            "SORT":100,
            "PLAN":null
        }
    ],
    "time":{
        "start":1714993577.419989,
        "finish":1714993577.621777,
        "duration":0.20178794860839844,
        "processing":0.027898073196411133,
        "date_start":"2024-05-06T14:06:17+02:00",
        "date_finish":"2024-05-06T14:06:17+02:00"
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_paysystem[]`](../sale/data-types.md) | An array of objects with information about the selected payment systems ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error handling

HTTP status: **400**, **403**

```json
{
    "error":"ACCESS_DENIED",
    "error_description":"Access denied!"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_CHECK_FAILURE` | Error validating incoming parameters (details can be found in the error description) | 400 ||
|| `ACCESS_DENIED` | Insufficient rights to obtain the list of payment systems | 403 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue exploring

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

