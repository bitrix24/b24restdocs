# Get a list of products in inventory management documents catalog.document.element.list

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: users with the "View product catalog" access permission

The method `catalog.document.element.list` returns product items associated with inventory management documents. Records are automatically limited by the available document types and the user's permissions on inventories.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) |  An array with a list of fields from [catalog_document_element](../../data-types.md#catalog_document_element) to be selected ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected product records in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_document_element](../../data-types.md#catalog_document_element) object.

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to,
- `>` — greater than,
- `<=` — less than or equal to,
- `<` — less than,
- `@` — IN, an array is passed as the value,
- `!@` — NOT IN, an array is passed as the value,
- `=` — equal, exact match, used by default,
- `!=` — not equal,
- `!` — not equal
||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the selected product records in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_document_element](../../data-types.md#catalog_document_element) object.

Possible values for `order`:
- `asc` — in ascending order,
- `desc` — in descending order 
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","docId","elementId","amount","storeFrom","storeTo"],"filter":{"docId":64},"order":{"id":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","docId","elementId","amount","storeFrom","storeTo"],"filter":{"docId":64},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.element.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once.
    // Use only for small selections (< 1000 items) due to high
    // memory load.

    try {
    const response = await $b24.callListMethod(
        'catalog.document.element.list',
        {
        select: [
            'id',
            'docId',
            'elementId',
            'amount',
            'storeFrom',
            'storeTo'
        ],
        filter: {
            docId: 64
        },
        order: {
            id: 'ASC'
        }
        },
        (progress) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
    console.error('Request failed', error)
    }

    // fetchListMethod: Retrieves data in parts using an iterator.
    // Use for large volumes of data for efficient memory consumption.

    try {
    const generator = $b24.fetchListMethod('catalog.document.element.list', {
        select: [
        'id',
        'docId',
        'elementId',
        'amount',
        'storeFrom',
        'storeTo'
        ],
        filter: {
        docId: 64
        },
        order: {
        id: 'ASC'
        }
    }, 'ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error) {
    console.error('Request failed', error)
    }

    // callMethod: Manual control of pagination through the start parameter.
    // Use for precise control over request batches.
    // Less efficient for large data than fetchListMethod.

    try {
    const response = await $b24.callMethod('catalog.document.element.list', {
        select: [
        'id',
        'docId',
        'elementId',
        'amount',
        'storeFrom',
        'storeTo'
        ],
        filter: {
        docId: 64
        },
        order: {
        id: 'ASC'
        }
    }, 0);
    const result = response.getData().result || [];
    for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
    console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.document.element.list',
                [
                    'select' => [
                        'id',
                        'docId',
                        'elementId',
                        'amount',
                        'storeFrom',
                        'storeTo',
                        'purchasingPrice'
                    ],
                    'filter' => [
                        'docId' => 64
                    ],
                    'order' => [
                        'id' => 'ASC'
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        $result->next();

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching document elements: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.element.list',
        {
            select: [
                'id',
                'docId',
                'elementId',
                'amount',
                'storeFrom',
                'storeTo'
            ],
            filter: {
                docId: 64
            },
            order: {
                id: 'ASC'
            }
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());

            result.next();
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.element.list',
        [
            'select' => [
                'id',
                'docId',
                'elementId',
                'amount',
                'storeFrom',
                'storeTo'
            ],
            'filter' => [
                'docId' => 64
            ],
            'order' => [
                'id' => 'ASC'
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
        "documentElements": [
            {
                "amount": 5,
                "docId": 64,
                "elementId": 312,
                "id": 148,
                "purchasingPrice": 1180,
                "storeFrom": null,
                "storeTo": 2
            },
            {
                "amount": 12,
                "docId": 64,
                "elementId": 420,
                "id": 149,
                "purchasingPrice": 560,
                "storeFrom": null,
                "storeTo": 2
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1759482402.511337,
        "finish": 1759482402.642843,
        "duration": 0.13150620460510254,
        "processing": 0.02694106101989746,
        "date_start": "2025-11-02T12:26:42+01:00",
        "date_finish": "2025-11-02T12:26:42+01:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The root element of the response ||
|| **documentElement**
[`object`](../../data-types.md#catalog_document_element) | An object with information about the document's products, the response structure depends on the `select` parameter ||
|| **total**
[`integer`](../../../data-types.md) | The total number of records ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_DOCUMENT_RIGHTS",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_DOCUMENT_RIGHTS` | Access denied | Insufficient rights for reading ||
|| `0` |  | Other processing errors ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-document-element-get-fields.md)
- [{#T}](./catalog-document-element-add.md)
- [{#T}](./catalog-document-element-update.md)
- [{#T}](./catalog-document-element-delete.md)