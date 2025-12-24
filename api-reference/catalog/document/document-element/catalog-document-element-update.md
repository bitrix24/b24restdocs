# Update Product in Inventory Document catalog.document.element.update

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: 
> - a user with "Create and Edit" permission on the document type in the request,
> - and "View and Select Warehouse" permission on the incoming or outgoing warehouse.

The method `catalog.document.element.update` modifies an existing item in the inventory document and returns the updated product data.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_document_element.id`](../../data-types.md#catalog_document_element) | Identifier of the document product, can be obtained using the [catalog.document.element.list](./catalog-document-element-list.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Fields to be modified (detailed description below) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **storeFrom**
[`catalog_store.id`](../../data-types.md#catalog_store) | Identifier of the source warehouse, can be obtained using the [catalog.store.list](../../store/catalog-store-list.md) method. Use for outgoing documents. If the parameter is not provided, the current value will be retained ||
|| **storeTo**
[`catalog_store.id`](../../data-types.md#catalog_store) | Identifier of the destination warehouse, can be obtained using the [catalog.store.list](../../store/catalog-store-list.md) method. Suitable for incoming and transfer documents. If the parameter is not provided, the current value will be retained ||
|| **amount**
[`double`](../../../data-types.md) | Quantity of the product in the document's accounting units. If the parameter is not provided, the current value will be retained ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchase price in the document's currency. If the parameter is not provided, the current value will be retained ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":148,"fields":{"amount":12,"purchasingPrice":1180,"storeTo":2}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":148,"fields":{"amount":12,"purchasingPrice":1180,"storeTo":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.element.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.element.update',
    		{
    			id: 148,
    			fields: {
    				amount: 12,
    				purchasingPrice: 1180,
    				storeTo: 2
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
                'catalog.document.element.update',
                [
                    'id' => 148,
                    'fields' => [
                        'amount' => 12,
                        'purchasingPrice' => 1180,
                        'storeTo' => 2,
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
        echo 'Error updating document element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.element.update',
        {
            id: 148,
            fields: {
                amount: 12,
                purchasingPrice: 1180,
                storeTo: 2
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
        'catalog.document.element.update',
        [
            'id' => 148,
            'fields' => [
                'amount' => 12,
                'purchasingPrice' => 1180,
                'storeTo' => 2,
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
        "documentElement": {
            "amount": 12,
            "docId": 64,
            "elementId": 312,
            "id": 148,
            "purchasingPrice": 1180,
            "storeFrom": null,
            "storeTo": 2
        }
    },
    "time": {
        "start": 1759482203.22173,
        "finish": 1759482203.33928,
        "duration": 0.11754989624023438,
        "processing": 0.02244114875793457,
        "date_start": "2025-11-02T12:23:23+01:00",
        "date_finish": "2025-11-02T12:23:23+01:00",
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
[`catalog_document_element`](../../data-types.md#catalog_document_element) | Updated product data of the document. ||
|| **time**
[`time`](../../../data-types.md#time) | Service information about the request processing time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_DOCUMENT_STATUS",
    "error_description": "Document not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_DOCUMENT_RIGHTS` | Access denied | Insufficient rights to modify the document or view warehouses ||
|| `ERROR_DOCUMENT_STATUS` | Document not found / Conducted document | Item not found, document deleted, unavailable, or already processed ||
|| `0` | Error of modifying new document | Internal error while saving ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-document-element-add.md)
- [{#T}](./catalog-document-element-delete.md)
- [{#T}](./catalog-document-element-list.md)
- [{#T}](./catalog-document-element-get-fields.md)