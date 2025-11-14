# Create Inventory Management Document catalog.document.add

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Create and edit" access permission for the required document type

The method `catalog.document.add` creates a new inventory management document.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](#fields) | Document fields ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **docType***
[`char`](../../data-types.md) | Document type. Possible values:
- `A` — receipt,
- `S` — stock adjustment,
- `M` — transfer between inventories,
- `R` — return,
- `D` — write-off.

Current document types can be retrieved using the method [catalog.enum.getStoreDocumentTypes](../enum/catalog-enum-get-store-document-types.md) ||
|| **currency***
[`char`](../../data-types.md) | Document currency in ISO 4217 format, for example `USD`. The value cannot be changed after creation.

To get a list of currencies with codes, use the method [crm.currency.list](../../crm/currency/crm-currency-list.md) ||
|| **responsibleId***
[`integer`](../../data-types.md) | Identifier of the responsible person ||
|| **siteId**
[`char`](../../data-types.md) | Site code to which the document relates. Default is `s1`.

This parameter is relevant for on-premise Bitrix, for cloud Bitrix the standard value is `s1` ||
|| **dateDocument**
[`datetime`](../../data-types.md) | Document date in ISO 8601 format ||
|| **title**
[`string`](../../data-types.md) | Document title ||
|| **commentary**
[`char`](../../data-types.md) | Document commentary ||
|| **total**
[`double`](../../data-types.md) | Total amount for the document products. The value is calculated automatically after processing but can be set manually ||
|| **docNumber**
[`string`](../../data-types.md) | Internal document number. If not provided, it is generated automatically ||
|| **createdBy**
[`integer`](../../data-types.md) | Identifier of the user who created the document. An administrator can specify any value; by default, it is filled with the current user ||
|#

{% note info "" %}

To fill in supplier data, use the method [catalog.documentcontractor.add](../documentcontractor/catalog-documentcontractor-add.md).
To fill in product data, use the method [catalog.document.element.add](./document-element/catalog-document-element-add.md).

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docType":"A","currency":"USD","responsibleId":29,"docNumber":"IN-00042","title":"Receipt from Supplier-1","commentary":"Planned inventory replenishment"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/catalog.document.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docType":"A","currency":"USD","responsibleId":29,"docNumber":"IN-00042","title":"Receipt from Supplier-1","commentary":"Planned inventory replenishment"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'catalog.document.add',
            {
                fields: {
                    docType: 'A',
                    currency: 'USD',
                    responsibleId: 29,
                    docNumber: 'IN-00042',
                    title: 'Receipt from Supplier-1',
                    commentary: 'Planned inventory replenishment',
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Created element with ID:', result);
        
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
            'catalog.document.add',
            [
                'fields' => [
                    'docType' => 'A',
                    'currency' => 'USD',
                    'responsibleId' => 29,
                    'docNumber' => 'IN-00042',
                    'title' => 'Receipt from Supplier-1',
                    'commentary' => 'Planned inventory replenishment'
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
        echo 'Error adding product row: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.add',
        {
            fields: {
                docType: 'A',
                currency: 'USD',
                responsibleId: 29,
                docNumber: 'IN-00042',
                title: 'Receipt from Supplier-1',
                commentary: 'Planned inventory replenishment'
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
        'catalog.document.add',
        [
            'fields' => [
                'docType' => 'A',
                'currency' => 'USD',
                'responsibleId' => 29,
                'docNumber' => 'IN-00042',
                'title' => 'Receipt from Supplier-1',
                'commentary' => 'Planned inventory replenishment'
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
        "document": {
            "commentary": "Planned inventory replenishment",
            "createdBy": 29,
            "currency": "USD",
            "dateCreate": "2025-10-30T11:19:38+02:00",
            "dateDocument": null,
            "dateModify": "2025-10-30T11:19:38+02:00",
            "dateStatus": "2025-10-30T11:19:38+02:00",
            "docNumber": "IN-00042",
            "docType": "A",
            "id": 11,
            "modifiedBy": 29,
            "responsibleId": 29,
            "siteId": "s1",
            "status": "N",
            "statusBy": null,
            "title": "Receipt from Supplier-1",
            "total": null
        }
    },
    "time": {
        "start": 1761805178,
        "finish": 1761805178.991429,
        "duration": 0.9914290904998779,
        "processing": 0,
        "date_start": "2025-10-30T09:19:38+02:00",
        "date_finish": "2025-10-30T09:19:38+02:00",
        "operating_reset_at": 1761805778,
        "operating": 0.2595658302307129
    }
}
```

{% note info "" %}

The document is created with the status `N` — draft. To process the document, use the method [catalog.document.conduct](./catalog-document-conduct.md) or [catalog.document.conductList](./catalog-document-conduct-list.md).

{% endnote %}

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md#catalog_document) | Root element of the response ||
|| **document**
[`object`](../data-types.md#catalog_document) | Object with data of the created document ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

HTTP code: **400**

```json
{
    "error": "0",
    "error_description": "DOC_TYPE isn't available"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Insufficient rights to save the document | The user does not have permission to create the required document type ||
|| `0` | DOC_TYPE isn't available | An invalid document type was provided ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-element/catalog-document-element-add.md)
- [{#T}](../documentcontractor/catalog-documentcontractor-add.md)
- [{#T}](./catalog-document-update.md)
- [{#T}](./catalog-document-conduct.md)