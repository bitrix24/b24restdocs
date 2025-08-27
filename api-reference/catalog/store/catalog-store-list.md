# Get a list of inventories by filter catalog.store.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of inventories based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select** 
[`array`](../../data-types.md)| An array of fields to select (see fields of the [catalog_store](../data-types.md#catalog_store) object) 
||
|| **filter** 
[`object`](../../data-types.md)| An object for filtering selected inventories in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_store](../data-types.md#catalog_store) object.

An additional prefix can be specified for the key to clarify the filter's behavior. Possible prefix values:
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
[`object`](../../data-types.md)| An object for sorting selected inventories in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_store](../data-types.md#catalog_store) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
||
|| **start** 
[`integer`](../../data-types.md)| This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page
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
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    const selectFields = [
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
    ];
    
    try {
        const response = await $b24.callListMethod(
            'catalog.store.list',
            {
                select: selectFields,
                filter: {
                    '@userId': [1, 2],
                    '<sort': 200,
                    'modifiedBy': 1,
                },
                order: {
                    id: 'desc',
                },
            },
            (progress) => { console.log('Progress:', progress) }
        );
        const items = response.getData() || [];
        for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
        const generator = $b24.fetchListMethod('catalog.store.list', {
            select: selectFields,
            filter: {
                '@userId': [1, 2],
                '<sort': 200,
                'modifiedBy': 1,
            },
            order: {
                id: 'desc',
            },
        }, 'id');
        for await (const page of generator) {
            for (const entity of page) { console.log('Entity:', entity); }
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
        const response = await $b24.callMethod('catalog.store.list', {
            select: selectFields,
            filter: {
                '@userId': [1, 2],
                '<sort': 200,
                'modifiedBy': 1,
            },
            order: {
                id: 'desc',
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
                        '@userId'    => [1, 2],
                        '<sort'      => 200,
                        'modifiedBy' => 1,
                    ],
                    'order'  => [
                        'id' => 'desc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching store list: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

HTTP status: **200**

```json
{
    "result": {
        "stores": [
            {
                "active": "Y",
                "address": "Main St. 52",
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
[`catalog_store[]`](../data-types.md#catalog_store) | An array of objects with information about the selected inventories ||
|| **total**
[`integer`](../../data-types.md#time) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

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
|| `200040300010` | Insufficient rights for reading ||
|| `0` | Other errors (e.g., fatal errors) || 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-store-add.md)
- [{#T}](./catalog-store-update.md)
- [{#T}](./catalog-store-get.md)
- [{#T}](./catalog-store-delete.md)
- [{#T}](./catalog-store-get-fields.md)