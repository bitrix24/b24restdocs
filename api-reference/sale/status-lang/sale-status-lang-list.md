# Get List of Localizations for sale.statusLang.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of localizations for order or delivery statuses.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to be selected (see fields of the [sale_status_lang](../data-types.md#sale_status_lang) object).

If not provided or an empty array is passed, all available localization fields will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected items in the shipment table in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_status_lang](../data-types.md#sale_status_lang) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol should not be included in the filter value. The search is conducted from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol must be included in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol must be included in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is absent in any position
- `!%=` — NOT LIKE (see description above)
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected statuses in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_status_lang](../data-types.md#sale_status_lang) object.

Possible values for order:

- `asc` — in ascending order
- `desc` — in descending order

||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.
 
The page size for results is always static: 50 records.
 
To select the second page of results, the value `50` must be passed. To select the third page of results, the value should be `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["statusId","lid","name","description"],"filter":{"statusId":"N","lid":"en"},"order":{"statusId":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.statuslang.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["statusId","lid","name","description"],"filter":{"statusId":"N","lid":"en"},"order":{"statusId":"asc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.statuslang.list
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.statuslang.list", {
            "select": [
                "statusId",
                "lid",
                "name",
                "description",
            ],
            "filter": {
                "statusId": "N",
                "lid": "en",
            },
            "order": {
                "statusId": "asc",
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
        'sale.statuslang.list',
        [
            'select' => [
                'statusId',
                'lid',
                'name',
                'description',
            ],
            'filter' => [
                'statusId' => 'N',
                'lid' => 'en',
            ],
            'order' => [
                'statusId' => 'asc',
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
    "result":{
        "statusLangs":[
            {
                "description":"Order accepted but not yet processed (for example, the order has just been created or payment is pending)",
                "lid":"en",
                "name":"Accepted, awaiting payment",
                "statusId":"N"
            }
        ]
    },
    "total":1,
    "time":{
        "start":1712656146.769524,
        "finish":1712656146.98067,
        "duration":0.21114587783813477,
        "processing":0.02018594741821289,
        "date_start":"2024-04-09T12:49:06+03:00",
        "date_finish":"2024-04-09T12:49:06+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **statusLangs**
[`sale_status_lang[]`](../data-types.md) | An array of objects containing information about the selected status localizations ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

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
|| `200040300010` | Insufficient permissions to read status localizations ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-status-lang-get-list-langs.md)
- [{#T}](./sale-status-lang-add.md)
- [{#T}](./sale-status-lang-delete-by-filter.md)
- [{#T}](./sale-status-lang-get-fields.md)