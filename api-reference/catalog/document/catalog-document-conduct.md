# Conduct inventory document catalog.document.conduct

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method:
> - a user with the "Conduct document" access permission for the document type in the request,
> - and "View and select warehouse" for the stock receipt or write-off warehouse.

The method `catalog.document.conduct` conducts the inventory document:
- the document status changes to `Y` — conducted,
- the inventory balances of products are updated according to the document's positions.

## Method parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Document identifier, can be obtained using the [catalog.document.list](./catalog-document-list.md) method ||
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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/catalog.document.conduct
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":142,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.conduct
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.conduct',
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
                'catalog.document.conduct',
                [
                    'id' => 142,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result === true) {
            echo 'Document conducted';
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error conducting document: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.conduct',
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
        'catalog.document.conduct',
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
        "start": 1762409135,
        "finish": 1762409136.304248,
        "duration": 1.3042480945587158,
        "processing": 1,
        "date_start": "2025-11-06T09:05:35+02:00",
        "date_finish": "2025-11-06T09:05:36+02:00",
        "operating_reset_at": 1762409735,
        "operating": 0.3091859817504883
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` if the document is conducted  ||
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
|| `0` | Failed to complete the action due to insufficient rights to view and select warehouses | No rights to work with the product warehouse from the document ||
|| `0` | Insufficient rights to save the document | No rights to the product catalog, inventory management, or no rights to conduct the document ||
|| `0` | Document not found | A non-existent document identifier was specified ||
|| `0` | Document conducting error: "error text" | The document contains incorrect data, for example, "Supplier not specified" ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue exploring

- [{#T}](./catalog-document-conduct-list.md)
- [{#T}](./catalog-document-cancel.md)
- [{#T}](./document-element/catalog-document-element-add.md)
- [{#T}](../documentcontractor/catalog-documentcontractor-add.md)