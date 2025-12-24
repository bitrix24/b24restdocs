# Add Product to Inventory Document catalog.document.element.add

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: 
> - a user with "Create and Edit" permission on the document type in the request,
> - and "View and Select Warehouse" permission on the incoming or outgoing warehouse.

The method `catalog.document.element.add` adds a product item to the inventory document.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Fields of the added product ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **docId***
[`catalog_document.id`](../../data-types.md#catalog_document) | Identifier of the inventory document, can be obtained using the [catalog.document.list](../catalog-document-list.md) method. The document must have a status of `N` — not processed ||
|| **elementId***
[`catalog_product.id`](../../data-types.md#catalog_product) | Identifier of the catalog product. The value can be obtained using the [catalog.product.*](../../product/index.md) methods ||
|| **storeFrom**
[`catalog_store.id`](../../data-types.md#catalog_store) | Identifier of the source warehouse, can be obtained using the [catalog.store.list](../../store/catalog-store-list.md) method. Used for outgoing documents ||
|| **storeTo**
[`catalog_store.id`](../../data-types.md#catalog_store) | Identifier of the receiving warehouse, can be obtained using the [catalog.store.list](../../store/catalog-store-list.md) method. Used for incoming and transfer documents ||
|| **amount**
[`double`](../../../data-types.md) | Quantity of the product. The value is specified in the units of the document ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchase price in the document's currency ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docId":64,"elementId":312,"storeTo":2,"amount":15,"purchasingPrice":1250.5}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docId":64,"elementId":312,"storeTo":2,"amount":15,"purchasingPrice":1250.5},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.element.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.element.add',
    		{
    			fields: {
    				docId: 64,
    				elementId: 312,
    				storeTo: 2,
    				amount: 15,
    				purchasingPrice: 1250.5
    			}
    		}
    	);

    	const result = response.getData().result;
    	console.log(result);
    }
    catch (error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.document.element.add',
                [
                    'fields' => [
                        'docId' => 64,
                        'elementId' => 312,
                        'storeTo' => 2,
                        'amount' => 15,
                        'purchasingPrice' => 1250.5,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding document element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.element.add',
        {
            fields: {
                docId: 64,
                elementId: 312,
                storeTo: 2,
                amount: 15,
                purchasingPrice: 1250.5
            }
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.element.add',
        [
            'fields' => [
                'docId' => 64,
                'elementId' => 312,
                'storeTo' => 2,
                'amount' => 15,
                'purchasingPrice' => 1250.5,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "documentElement": {
            "amount": 15,
            "docId": 64,
            "elementId": 312,
            "id": 148,
            "purchasingPrice": 1250.5,
            "storeFrom": null,
            "storeTo": 2
        }
    },
    "time": {
        "start": 1759482001.102334,
        "finish": 1759482001.215487,
        "duration": 0.11315321922302246,
        "processing": 0.018451929092407227,
        "date_start": "2025-11-02T12:20:01+01:00",
        "date_finish": "2025-11-02T12:20:01+01:00",
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
|| **documentElement**
[`catalog_document_element`](../../data-types.md#catalog_document_element) | Object with information about the added product ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Code: **400**

```json
{
    "error": "ERROR_DOCUMENT_STATUS",
    "error_description": "Conducted document"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_DOCUMENT_RIGHTS` | Access denied | Insufficient rights on the document or one of the specified warehouses ||
|| `ERROR_DOCUMENT_STATUS` | Document not found / Conducted document | Document not found, unavailable, or already processed ||
|| `0` | Error of adding new document element | Internal error while saving the item ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-document-element-update.md)
- [{#T}](./catalog-document-element-delete.md)
- [{#T}](./catalog-document-element-list.md)
- [{#T}](./catalog-document-element-get-fields.md)