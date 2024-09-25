# Get a List of Trade Catalogs catalog.catalog.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method returns a list of trade catalogs.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | 
An array of fields to select (see fields of the object [catalog_catalog](../data-types.md#catalog_catalog)).

If the array is not provided or an empty array is passed, all available fields of the trade catalogs will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering selected records in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_catalog](../data-types.md#catalog_catalog). 

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value should not be passed. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol must be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value should not be passed. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol must be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal ||
|| **order**
[`object`](../../data-types.md) | 
An object for sorting selected trade catalogs in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_catalog](../data-types.md#catalog_catalog).

Possible values for `order`:
- `asc` — ascending order
- `desc` — descending order
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for pagination control.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number
||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["iblockId","iblockTypeId","id","lid","name","productIblockId","skuPropertyId","subscription","vatId"],"filter":{">id":10,"@vatId":[1,2],"skuPropertyId":121},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.catalog.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["iblockId","iblockTypeId","id","lid","name","productIblockId","skuPropertyId","subscription","vatId"],"filter":{">id":10,"@vatId":[1,2],"skuPropertyId":121},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.catalog.list
    ```

- JS

    ```js
    BX24.callMethod(
        "catalog.catalog.list", {
            "select": [
                "iblockId",
                "iblockTypeId",
                "id",
                "lid",
                "name",
                "productIblockId",
                "skuPropertyId",
                "subscription",
                "vatId"
            ],
            "filter": {
                ">id": 10,
                "@vatId": [1, 2],
                "skuPropertyId": 121,
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
        'catalog.catalog.list',
        [
            'select' => [
                'iblockId',
                'iblockTypeId',
                'id',
                'lid',
                'name',
                'productIblockId',
                'skuPropertyId',
                'subscription',
                'vatId'
            ],
            'filter' => [
                '>id' => 10,
                '@vatId' => [1, 2],
                'skuPropertyId' => 121,
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
    "result": {
        "catalogs": [
            {
                "iblockId": 24,
                "iblockTypeId": 0,
                "id": 24,
                "lid": "s1",
                "name": "Trade Catalog CRM (offers)",
                "productIblockId": 23,
                "skuPropertyId": 97,
                "subscription": "N",
                "vatId": 1
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1716796276.902035,
        "finish": 1716796277.31202,
        "duration": 0.4099850654602051,
        "processing": 0.01759815216064453,
        "date_start": "2024-05-27T10:51:16+02:00",
        "date_finish": "2024-05-27T10:51:17+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **catalogs**
[`catalog_catalog[]`](../data-types.md#catalog_catalog) | An array of objects with information about the selected trade catalogs ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
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
|| `200040300010` | Insufficient permissions to read trade catalogs
|| 
|| `200040300030` | Insufficient permissions to read trade catalogs
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-catalog-add.md)
- [{#T}](./catalog-catalog-update.md)
- [{#T}](./catalog-catalog-get.md)
- [{#T}](./catalog-catalog-is-offers.md)
- [{#T}](./catalog-catalog-delete.md)
- [{#T}](./catalog-catalog-get-fields.md)