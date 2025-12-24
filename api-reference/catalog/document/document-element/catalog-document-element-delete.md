# Delete Item from Inventory Document catalog.document.element.delete

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: 
> - a user with "Create and edit" permission on the document type in the request,
> - and "View and select warehouse" permission on the incoming or outgoing warehouse.

The method `catalog.document.element.delete` removes an item from the inventory document. The document must be accessible to the user and have a status of `N` — not processed.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_document_element.id`](../../data-types.md#catalog_document_element) | Identifier of the item record in the document, can be obtained using the [catalog.document.element.list](./catalog-document-element-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":148}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":148,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.element.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.element.delete',
    		{ id: 148 }
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
                'catalog.document.element.delete',
                [
                    'id' => 148,
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
        echo 'Error deleting document element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.element.delete',
        { id: 148 },
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
        'catalog.document.element.delete',
        [
            'id' => 148,
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
    "result": true,
    "time": {
        "start": 1759482302.413852,
        "finish": 1759482302.497163,
        "duration": 0.08331108093261719,
        "processing": 0.01520085334777832,
        "date_start": "2025-11-02T12:25:02+02:00",
        "date_finish": "2025-11-02T12:25:02+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

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
|| `ERROR_DOCUMENT_RIGHTS` | Access denied | Insufficient rights to work with the document or warehouses ||
|| `ERROR_DOCUMENT_STATUS` | Document not found / Conducted document | Item not found, document deleted or already processed ||
|| `0` | Error of deleting document | Failed to delete the record ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-document-element-add.md)
- [{#T}](./catalog-document-element-update.md)
- [{#T}](./catalog-document-element-list.md)
- [{#T}](./catalog-document-element-get-fields.md)