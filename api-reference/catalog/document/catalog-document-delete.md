# Delete inventory management document catalog.document.delete

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: 
> - a user with the "Delete document" access permission for the document type in the request,
> - and "View and select inventory" for the stock receipt or write-off.

The method `catalog.document.delete` removes an inventory management document. 

## Method parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Document identifier, which can be obtained using the [catalog.document.list](./catalog-document-list.md) method ||
|#

## Code examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":142}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":142,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.delete',
    		{ id: 142 }
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
                'catalog.document.delete',
                [
                    'id' => 142,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result === true) {
            echo 'Document deleted';
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting document: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.delete',
        { id: 142 },
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
        'catalog.document.delete',
        [
            'id' => 142,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response handling

HTTP code: **200**

```json
{
    "result": true,
    "time": {
        "start": 1761908531,
        "finish": 1761908531.935914,
        "duration": 0.9359140396118164,
        "processing": 0,
        "date_start": "2025-10-31T14:02:11+02:00",
        "date_finish": "2025-10-31T14:02:11+02:00",
        "operating_reset_at": 1761909131,
        "operating": 0
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` if the document is deleted. 

If the response contains `null` — the document cannot be deleted because it is processed. First, cancel the processing of the document using the [catalog.document.cancel](./catalog-document-cancel.md) method ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

HTTP code: **400**

```json
{
    "error": "0",
    "error_description": "Document not found"
}
```

### Possible error codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Insufficient permissions to save the document | Insufficient permissions to work with the document or inventories ||
|| `0` | Document not found | A non-existent document identifier was provided ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue exploring

- [{#T}](./catalog-document-delete-list.md)
- [{#T}](./catalog-document-cancel.md)
- [{#T}](./catalog-document-list.md)