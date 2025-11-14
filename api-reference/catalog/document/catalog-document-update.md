# Update Inventory Document catalog.document.update

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create and edit" access permission for the required document type

The method `catalog.document.update` modifies the fields of an existing inventory document.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Document identifier, can be obtained using the [catalog.document.list](./catalog-document-list.md) method ||
|| **fields***
[`object`](#fields) | Document fields ([detailed description](#fields)) ||
|#

### Fields Parameter {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **responsibleId**
[`integer`](../../data-types.md) | Identifier of the responsible person ||
|| **dateModify**
[`datetime`](../../data-types.md) | You can provide your own modification date. By default, it is the current date ||
|| **dateDocument**
[`datetime`](../../data-types.md) | Document date ||
|| **total**
[`double`](../../data-types.md) | Total amount for the document products. Automatically recalculated after changing product items ||
|| **commentary**
[`char`](../../data-types.md) | Commentary for the document ||
|| **title**
[`string`](../../data-types.md) | Document title ||
|| **docNumber**
[`string`](../../data-types.md) | Internal document number ||
|| **modifiedBy**
[`integer`](../../data-types.md) | Identifier of the user who modified the document. An administrator can specify any value; by default, it is filled with the current user ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":142,"fields":{"title":"Receipt from Vendor-1 (correction)","commentary":"Updated responsible person","responsibleId":21}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":142,"fields":{"title":"Receipt from Vendor-1 (correction)","commentary":"Updated responsible person","responsibleId":21},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'catalog.document.update',
            {
                id: 142,
                fields: {
                    title: 'Receipt from Vendor-1 (correction)',
                    commentary: 'Updated responsible person',
                    responsibleId: 21,
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Updated document with ID:', result);
        
        processResult(result);
    }
    catch( error )
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
                'catalog.document.update',
                [
                    'id' => 142,
                    'fields' => [
                        'title' => 'Receipt from Vendor-1 (correction)',
                        'commentary' => 'Updated responsible person',
                        'responsibleId' => 21
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating document: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.update',
        {
            id: 142,
            fields: {
                title: 'Receipt from Vendor-1 (correction)',
                commentary: 'Updated responsible person',
                responsibleId: 21
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
        'catalog.document.update',
        [
            'id' => 142,
            'fields' => [
                'title' => 'Receipt from Vendor-1 (correction)',
                'commentary' => 'Updated responsible person',
                'responsibleId' => 21
            ]
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
        "document": {
            "commentary": "Updated responsible person",
            "createdBy": 29,
            "currency": "USD",
            "dateCreate": "2025-10-30T11:19:38+02:00",
            "dateDocument": null,
            "dateModify": "2025-10-30T11:33:42+02:00",
            "dateStatus": "2025-10-30T11:19:38+02:00",
            "docNumber": "IN-00042",
            "docType": "A",
            "id": 11,
            "modifiedBy": 29,
            "responsibleId": 21,
            "siteId": "s1",
            "status": "N",
            "statusBy": null,
            "title": "Receipt from Vendor-1 (correction)",
            "total": null
        }
    },
    "time": {
        "start": 1761806022,
        "finish": 1761806022.36133,
        "duration": 0.3613300323486328,
        "processing": 0,
        "date_start": "2025-10-30T09:33:42+02:00",
        "date_finish": "2025-10-30T09:33:42+02:00",
        "operating_reset_at": 1761806622,
        "operating": 0.17665815353393555
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md#catalog_document) | Root element of the response ||
|| **document**
[`object`](../data-types.md#catalog_document) | Object with updated document data ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

HTTP Code: **400**

```json
{
    "error": "0",
    "error_description": "Document not found."
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Insufficient permissions to save the document | The user does not have permission to edit the document of the required type or a document with such an identifier does not exist ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-element/catalog-document-element-update.md)
- [{#T}](../documentcontractor/catalog-documentcontractor-add.md)
- [{#T}](./catalog-document-cancel.md)
- [{#T}](./catalog-document-add.md)
- [{#T}](./catalog-document-conduct.md)