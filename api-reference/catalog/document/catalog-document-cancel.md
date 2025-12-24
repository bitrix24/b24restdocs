# Cancel Document of Inventory Accounting catalog.document.cancel

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method:
> - a user with the "Cancel Conduct" permission for the document type in the request,
> - and "View and Select Warehouse" permission for the incoming or outgoing warehouse.

The method `catalog.document.cancel` cancels the conduct of the inventory accounting document:
- the document status changes to `C` — canceled,
- the inventory balances of goods are updated according to the document's positions.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_document.id`](../data-types.md#catalog_document) | Identifier of the document, which can be obtained using the [catalog.document.list](./catalog-document-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":142}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.cancel
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":142,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.cancel
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.cancel',
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
                'catalog.document.cancel',
                [
                    'id' => 142,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result === true) {
            echo 'Document cancellation succeeded';
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error cancelling document: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.cancel',
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
        'catalog.document.cancel',
        [
            'id' => 142,
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
        "start": 1762411074,
        "finish": 1762411074.877169,
        "duration": 0.8771688938140869,
        "processing": 0,
        "date_start": "2025-11-06T09:37:54+01:00",
        "date_finish": "2025-11-06T09:37:54+01:00",
        "operating_reset_at": 1762411674,
        "operating": 0.2729671001434326
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` if the document conduct was canceled  ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

HTTP Code: **400**

```json
{
    "error": "0",
    "error_description": "Error cancelling document conduct: Document has not been conducted yet"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Insufficient permissions to save the document | No permissions for the product catalog, inventory accounting, no permission to cancel document conduct, or a non-existent document identifier was specified ||
|| `0` | Could not complete the action as you do not have sufficient permissions to view and select warehouses | No permissions to work with the product warehouse from the document ||
|| `0` | Error cancelling document conduct: Document has not been conducted yet | Cannot cancel document conduct if it is not in the conducted status ||
|| `0` | Inventory accounting is not available on your plan | Inventory accounting is not available on your plan ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-conduct.md)
- [{#T}](./catalog-document-cancel-list.md)
- [{#T}](./catalog-document-list.md)