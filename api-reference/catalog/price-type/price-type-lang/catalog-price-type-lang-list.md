# Get a List of Translations for Price Type Names by Filter catalog.priceTypeLang.list

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of translations for price type names.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | 
An array of fields to select (see fields of the [catalog_price_type_lang](../../data-types.md#catalog_price_type_lang) object).

If the array is not provided or an empty array is passed, all available fields of the price type name translations will be selected.
||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected price type name translations in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_price_type_lang](../../data-types.md#catalog_price_type_lang) object.

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal ||
|| **order**
[`object`](../../../data-types.md) | 
An object for sorting the selected price type name translations in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_price_type_lang](../../data-types.md#catalog_price_type_lang) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page
||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","lang"],"order":{"lang":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceTypeLang.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","lang"],"order":{"lang":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceTypeLang.list
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.priceTypeLang.list',
        {
            select:[
                'name',
                'lang'
            ],
            order:{
                'lang': 'ASC'
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
            result.next();
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.priceTypeLang.list',
        [
            'select' => [
                'name',
                'lang'
            ],
            'order' => [
                'lang' => 'ASC'
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
    "result": {
        "priceTypeLangs": [
            {
                "lang": "ar",
                "name": "PRICE"
            },
            {
                "lang": "en",
                "name": "Retail price"
            },
            {
                "lang": "kz",
                "name": "Base Price"
            },
            {
                "lang": "de",
                "name": "Retail price"
            }
        ]
    },
    "total": 4,
    "time": {
        "start": 1733844533.51186,
        "finish": 1733844533.81483,
        "duration": 0.302963972091675,
        "processing": 0.0111780166625977,
        "date_start": "2024-12-10T17:28:53+02:00",
        "date_finish": "2024-12-10T17:28:53+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **priceTypeLangs**
[`catalog_price_type_lang[]`](../../data-types.md#catalog_price_type_lang) | An array of objects with information about the selected price type name translations ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-lang-add.md)
- [{#T}](./catalog-price-type-lang-update.md)
- [{#T}](./catalog-price-type-lang-get.md)
- [{#T}](./catalog-price-type-lang-delete.md)
- [{#T}](./catalog-price-type-lang-get-languages.md)
- [{#T}](./catalog-price-type-lang-get-fields.md)