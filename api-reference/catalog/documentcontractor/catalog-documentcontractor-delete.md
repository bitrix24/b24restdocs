# Remove Vendor Binding from Document catalog.documentcontractor.delete

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the following permissions:
> — "View" and "Create and Edit" for the document type "Incoming",
> — "View Inventory Accounting Section"
> — "View Product Catalog"    

The method `catalog.documentcontractor.delete` removes the vendor from the inventory accounting document.

## Method Parameters  

{% include [Note on required parameters](../../../_includes/required.md) %}  

#|
|| **Name**
`type` | **Description** || 
|| **id***
[`catalog_documentcontractor.id`](../data-types.md#catalog_documentcontractor) | Identifier for the vendor binding to the document. It can be obtained using the method [catalog.documentcontractor.list](./catalog-documentcontractor-list.md) ||  
|#  

## Code Examples  

{% include [Note on examples](../../../_includes/examples.md) %}  

{% list tabs %}

- cURL (Webhook)

    ```bash 
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":42}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.documentcontractor.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":42,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.documentcontractor.delete
    ```

- JS

    ```js 
    try
    {
        const response = await $b24.callMethod(
            'catalog.documentcontractor.delete',
            { id: 42 }
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
                'catalog.documentcontractor.delete',
                [
                    'id' => 42,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result === true) {
            echo 'Contractor binding deleted';
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting contractor binding: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.documentcontractor.delete',
        { id: 42 },
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
        'catalog.documentcontractor.delete',
        [
            'id' => 42,
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

## Returned Data  

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md)| Root element of the response, contains `true` if the binding was successfully deleted.  
If the response contains `null` — the binding was not found or the user does not have permission to modify the document ||  
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time || 
|#  

## Error Handling  

{% include notitle [error handling](../../../_includes/error-info.md) %}  

HTTP Code: **400**

```json
{
    "error": "0",
    "error_description": "Binding was not found"
}
```

### Possible Error Codes  

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Binding was not found | The specified binding identifier does not exist ||  
|| `0` | Access denied | Insufficient permissions to modify the inventory accounting document ||  
|| `0` | Contractors should be provided by CRM | The CRM module is not active as a vendor for contractors ||  
|#  

{% include [System Errors](../../../_includes/system-errors.md) %}  

## Continue Learning  

- [{#T}](./catalog-documentcontractor-list.md)  
- [{#T}](./catalog-documentcontractor-add.md)  
- [{#T}](./catalog-documentcontractor-get-fields.md)  