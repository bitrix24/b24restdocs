# Conduct several inventory management documents catalog.document.conductList

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method:
> - a user with the "Conduct document" access permission for the document type in the request,
> - and "View and select warehouse" for the stock receipt or write-off warehouse.

The method `catalog.document.conductList` conducts a group of inventory management documents:
- the status of the documents is changed to `Y` — conducted,
- the inventory balances of products are updated according to the document positions.

Access permissions are checked for each document in the request.

## Method parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **documentIds***
[`array`](../../data-types.md) | List of document identifiers, which can be obtained using the [catalog.document.list](./catalog-document-list.md) method ||
|#

## Code examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentIds":[142,143,144]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.conductList
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentIds":[142,143,144],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.conductList
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.conductList',
    		{
    			documentIds: [142, 143, 144]
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
                'catalog.document.conductList',
                [
                    'documentIds' => [142, 143, 144],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result === true) {
            echo 'Documents conducted';
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error conducting documents: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.conductList',
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
        'catalog.document.conductList',
        [
            'documentIds' => [142, 143, 144],
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
        "start": 1762410455,
        "finish": 1762410455.743473,
        "duration": 0.7434730529785156,
        "processing": 0,
        "date_start": "2025-11-06T09:27:35+01:00",
        "date_finish": "2025-11-06T09:27:35+01:00",
        "operating_reset_at": 1762411055,
        "operating": 0.2910308837890625
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | The root element of the response, contains `true` if all documents were conducted without errors. If at least one document could not be conducted, the method will return an error in the response `error` / `error_description`. Documents that were successfully processed will remain in the "Conducted" status ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

HTTP code: **400**

```json
{
    "error": "0",
    "error_description": "An error occurred while conducting the document "Receipt 33": Incorrect quantity of product #6907 (Monster Energy) in the inventory management document"
}
```

### Possible error codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | An error occurred while conducting the document “#document name”: “error text” | The document contains incorrect data, for example “Supplier not specified” ||
|| `0` | Document conducting error: Insufficient permissions to save the document | No access to the product catalog, inventory management, or no permission to conduct the document ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue exploring

- [{#T}](./catalog-document-conduct.md)
- [{#T}](./catalog-document-cancel-list.md)
- [{#T}](./catalog-document-list.md)
- [{#T}](./document-element/catalog-document-element-add.md)
- [{#T}](../documentcontractor/catalog-documentcontractor-add.md)