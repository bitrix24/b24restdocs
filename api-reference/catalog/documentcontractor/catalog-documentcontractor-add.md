# Add Vendor to Inventory Document catalog.documentcontractor.add

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: user with permissions:
> — "View" and "Create and edit" on document type "Incoming",
> — "View Inventory" section
> — "View Product Catalog"  

The method `catalog.documentcontractor.add` creates a binding of a vendor, contact, or company to an inventory document.

## Method Parameters  

{% include [Note on required parameters](../../../_includes/required.md) %}  

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](#fields) | Binding fields ([detailed description](#fields)) ||
|# 

## Parameter fields {#fields}  

{% include [Note on required parameters](../../../_includes/required.md) %}  

#|
|| **Name**
`type` | **Description** || 
|| **documentId*** 
[`catalog_document.id`](../data-types.md#catalog_document) | Identifier of the inventory document of type "Incoming" `A`.  
Can be obtained using the method [catalog.document.list](../document/catalog-document-list.md) ||  
|| **entityTypeId***  
[`integer`](../../crm/data-types.md#object_type) | Type of CRM object:  
`3` — contact 
`4` — company ||  
|| **entityId***  
[`integer`](../../data-types.md) | Identifier of the CRM entity, contact, or company from the "Vendor" category.
 
To obtain vendor identifiers:  
1. Get the category identifier with the code `CATALOG_CONTRACTOR_CONTACT` for contacts or `CATALOG_CONTRACTOR_COMPANY` for companies using the method [crm.category.list](../../crm/universal/category/crm-category-list.md).  
2. Use the obtained `categoryId` in the filter of the request [crm.item.list](../../crm/universal/crm-item-list.md) ||  
|#  

## Code Examples  

{% include [Note on examples](../../../_includes/examples.md) %}  

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"documentId":42,"entityTypeId":3,"entityId":101}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.documentcontractor.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"documentId":42,"entityTypeId":3,"entityId":101},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.documentcontractor.add
    ```

- JS

    ```js  
    try
    {
        const response = await $b24.callMethod(
            'catalog.documentcontractor.add',
            {
                fields: {
                    documentId: 42,
                    entityTypeId: 3,
                    entityId: 101
                }
            }
        );

        const result = response.getData().result;
        console.log('Created binding:', result);
    }
    catch (error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php  
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.documentcontractor.add',
                [
                    'fields' => [
                        'documentId' => 42,
                        'entityTypeId' => 3,
                        'entityId' => 101
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result) {
            echo 'Success: ' . print_r($result, true);
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding contractor binding: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.documentcontractor.add',
        {
            fields: {
                documentId: 42,
                entityTypeId: 3,
                entityId: 101
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
        'catalog.documentcontractor.add',
        [
            'fields' => [
                'documentId' => 42,
                'entityTypeId' => 3,
                'entityId' => 101
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP code: **200**

```json
{
    "result": {
        "documentContractor": {
            "documentId": 73,
            "entityId": 2185,
            "entityTypeId": 3,
            "id": 15
        }
    },
    "time": {
        "start": 1766469835,
        "finish": 1766469835.824666,
        "duration": 0.8246660232543945,
        "processing": 0,
        "date_start": "2025-12-23T09:03:55+01:00",
        "date_finish": "2025-12-23T09:03:55+01:00",
        "operating_reset_at": 1766470435,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **documentContractor**
[`catalog_documentContractor`](../data-types.md#catalog_documentContractor) | Object with data of the created vendor binding to the inventory document ||  
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling 

{% include notitle [error handling](../../../_includes/error-info.md) %}  

HTTP code: **400**

```json
{
    "error": "0",
    "error_description": "Store document was not found"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Store document was not found | The specified document identifier does not exist or is inaccessible ||  
|| `0` | Type of store document is wrong | The document is not of type "Incoming" `A` ||  
|| `0` | Unable to edit conducted document | The document has already been processed and cannot be changed ||  
|| `0` | Wrong entity type id | An invalid `entityTypeId` was provided, must be 3 or 4 ||  
|| `0` | Wrong entity id | An invalid or non-existent `entityId` was specified ||  
|| `0` | This contractor has been already bound to this document | Such a binding already exists ||  
|| `0` | This document already has a Company contractor | A company is already bound to the document. Re-binding companies is prohibited ||  
|| `0` | Access denied | Insufficient permissions to modify the document ||  
|| `0` | Contractors should be provided by CRM | The CRM module is not active as a vendor for contractors ||  
|#

{% include [System Errors](../../../_includes/system-errors.md) %}  

## Continue Learning

- [{#T}](./catalog-documentcontractor-list.md)  
- [{#T}](./catalog-documentcontractor-delete.md)  
- [{#T}](./catalog-documentcontractor-get-fields.md)  