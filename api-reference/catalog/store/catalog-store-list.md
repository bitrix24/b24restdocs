# Get a List of Warehouses by Filter catalog.store.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of warehouses based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select** 
[`array`](../../data-types.md)| An array of fields to select (see fields of the [catalog_store](../data-types.md#catalog_store) object) 
||
|| **filter** 
[`object`](../../data-types.md)| An object for filtering selected warehouses in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_store](../data-types.md#catalog_store) object.

An additional prefix can be specified for the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md)| An object for sorting selected warehouses in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_store](../data-types.md#catalog_store) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
||
|| **start** 
[`integer`](../../data-types.md)| This parameter is used for pagination control.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
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
    -d '{"select":["id","title","active","address","description","gpsN","gpsS","imageId","dateModify","dateCreate","userId","modifiedBy","phone","schedule","xmlId","sort","email","issuingCenter","code"],"filter":{"@userId":[1,2],"<sort":200,"modifiedBy":1},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.store.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","active","address","description","gpsN","gpsS","imageId","dateModify","dateCreate","userId","modifiedBy","phone","schedule","xmlId","sort","email","issuingCenter","code"],"filter":{"@userId":[1,2],"<sort":200,"modifiedBy":1},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.store.list
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.store.list',
            {
                select:[
                    'id',
                    'title',
                    'active',
                    'address',
                    'description',
                    'gpsN',
                    'gpsS',
                    'imageId',
                    'dateModify',
                    'dateCreate',
                    'userId',
                    'modifiedBy',
                    'phone',
                    'schedule',
                    'xmlId',
                    'sort',
                    'email',
                    'issuingCenter',
                    'code',
                ],
                filter:{
                    '@userId': [1, 2],
                    '<sort': 200,
                    'modifiedBy': 1,
                },
                order:{
                    id: 'desc',
                },
            },
            function(result)
            {
                if(result.error()) {
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
        'catalog.store.list',
        [
            'select' => [
                'id',
                'title',
                'active',
                'address',
                'description',
                'gpsN',
                'gpsS',
                'imageId',
                'dateModify',
                'dateCreate',
                'userId',
                'modifiedBy',
                'phone',
                'schedule',
                'xmlId',
                'sort',
                'email',
                'issuingCenter',
                'code',
            ],
            'filter' => [
                '@userId' => [1, 2],
                '<sort' => 200,
                'modifiedBy' => 1,
            ],
            'order' => [
                'id' => 'desc',
            ],
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
        "stores": [
            {
                "active": "Y",
                "address": "Moscow Ave. 52",
                "code": "store_1",
                "dateCreate": "2024-10-18T16:30:45+02:00",
                "dateModify": "2024-10-21T14:29:06+02:00",
                "description": "Description",
                "email": "test@test.com",
                "gpsN": 54.71411,
                "gpsS": 21.56675,
                "id": 1,
                "imageId": {
                    "id": 1,
                    "url": "\/upload\/iblock\/6f1\/bkm7jmwso31wisk423gtp28iagy2e8v0\/test.jpeg"
                },
                "issuingCenter": "N",
                "modifiedBy": 1,
                "phone": "+1 (495) 212 85 06",
                "schedule": "Mon.-Fri. from 9:00 to 20:00, Sat.-Sun. from 11:00 to 18:00",
                "sort": 100,
                "title": "Warehouse 1",
                "userId": 1,
                "xmlId": null
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1729524388.951684,
        "finish": 1729524389.466658,
        "duration": 0.5149741172790527,
        "processing": 0.04066300392150879,
        "date_start": "2024-10-21T18:26:28+02:00",
        "date_finish": "2024-10-21T18:26:29+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **stores**
[`catalog_store[]`](../data-types.md#catalog_store) | Array of objects with information about the selected warehouses ||
|| **total**
[`integer`](../../data-types.md#time) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions for reading ||
|| `0` | Other errors (e.g., fatal errors) || 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-store-add.md)
- [{#T}](./catalog-store-update.md)
- [{#T}](./catalog-store-get.md)
- [{#T}](./catalog-store-delete.md)
- [{#T}](./catalog-store-get-fields.md)