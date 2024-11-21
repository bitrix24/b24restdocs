# Get the list of margins catalog.extra.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of margins based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select** 
[`array`](../../data-types.md)| An array with the list of fields to select (see the fields of the [catalog_extra](../data-types.md#catalog_extra) object).

If the array is not provided or an empty array is passed, all available fields of the margin type will be selected.
||
|| **filter** 
[`object`](../../data-types.md)| An object for filtering the selected margins in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_extra](../data-types.md#catalog_extra) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
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
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md)| An object for sorting the selected margins in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_extra](../data-types.md#catalog_extra) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
||
|| **start** 
[`integer`](../../data-types.md)| This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` is the number of the desired page
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
    -d '{"select":["id","percentage"],"filter":{">percentage":5},"order":{"id":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.extra.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","percentage"],"filter":{">percentage":5},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.extra.list
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.extra.list',
        {
            select:[
                'id',
                'percentage'
            ],
            filter:{
                '>percentage': 5
            },
            order:{
                'id': 'ASC'
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
        'catalog.extra.list',
        [
            'select' => [
                'id',
                'percentage'
            ],
            'filter' => [
                '>percentage' => 5
            ],
            'order' => [
                'id' => 'ASC'
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
    "extras": [
        {
            "id": 1,
            "percentage": 5.10
        }
    ],
    "total": 1,
    "time": {
        "start": 1712326352.63409,
        "finish": 1712326352.8319,
        "duration": 0.197818040847778,
        "processing": 0.00833678245544434,
        "date_start": "2024-10-24T16:24:46+02:00",
        "date_finish": "2024-10-24T16:24:46+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **extras**
[`catalog_extra[]`](../data-types.md#catalog_extra) | Array of objects with information about the selected margins ||
|| **total**
[`integer`](../../data-types.md#time) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read
||
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-extra-get.md)
- [{#T}](./catalog-extra-get-fields.md)