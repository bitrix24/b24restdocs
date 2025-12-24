# Canceling Multiple Documents catalog.document.cancelList

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method:
> - a user with the "Canceling" access permission on the document type in the request,
> - and "View and select warehouse" on the incoming or outgoing warehouse.

The method `catalog.document.cancelList` cancels the processing of a group of inventory documents:
- the status of the documents is changed to `C` — canceled,
- the inventory balances of the goods are updated according to the positions of the documents.

Access permissions are checked for each document in the request.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **documentIds***
[`catalog_document.id[]`](../data-types.md#catalog_document) | A list of document identifiers, which can be obtained using the [catalog.document.list](./catalog-document-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentIds":[142,143,144]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.cancelList
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentIds":[142,143,144],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.cancelList
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.cancelList',
    		{ documentIds: [142, 143, 144] }
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
                'catalog.document.cancelList',
                [
                    'documentIds' => [142, 143, 144],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result === true) {
            echo 'Documents unconfirmed';
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error canceling documents: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.cancelList',
        { documentIds: [142, 143, 144] },
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
        'catalog.document.cancelList',
        [
            'documentIds' => [142, 143, 144],
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
    "result": true,
    "time": {
        "start": 1762411998,
        "finish": 1762411998.634683,
        "duration": 0.6346828937530518,
        "processing": 0,
        "date_start": "2025-11-06T09:53:18+01:00",
        "date_finish": "2025-11-06T09:53:18+01:00",
        "operating_reset_at": 1762412598,
        "operating": 0.30604004859924316
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | The root element of the response, contains `true` if all documents were canceled without errors. If at least one document could not be canceled, the method will return an error in the `error` / `error_description` response. Documents that were successfully processed will remain in the "Canceled" status ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

HTTP Code: **400**

```json
{
    "error": "0",
    "error_description": "An error occurred while canceling the document "Receipt 35": The document has not been processed yet"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | An error occurred while canceling the document "document name": Insufficient permissions to save the document | No access permission to the product catalog, inventory accounting, or no permission to process the document ||
|| `0` | Error canceling the document: Document not found | A non-existent identifier was specified ||
|| `0` | An error occurred while canceling the document "document name": The document has not been processed yet | Cannot cancel the processing of a document if it is not in the processed status ||
|| `0` | Inventory accounting is not available on your plan | Inventory accounting is not available on your plan ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-cancel.md)
- [{#T}](./catalog-document-conduct-list.md)
- [{#T}](./catalog-document-list.md)