# Update Custom Field Values for Inventory Documents `catalog.userfield.document.update`

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Create and Edit" access permission for the required document type

The method `catalog.userfield.document.update` updates the values of custom fields for inventory documents.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **documentId***
[`catalog_document.id`](../data-types.md#catalog_document) | Identifier of the inventory document. The identifier can be obtained using the [catalog.document.list](../document/catalog-document-list.md) method ||
|| **fields***
[`object`](#fields) | Fields to update ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **documentType***
[`string`](../../data-types.md) | Type of the inventory document.

Allowed values: [types of inventory documents](../enum/catalog-enum-get-store-document-types.md) ||
|| **fieldN**
[`mixed`](../../data-types.md) | Value of the custom field, where `N` is the identifier of the custom field, for example, `field287`.

Identifiers and settings for custom fields can be obtained using the [userfieldconfig.list](../../crm/universal/userfieldconfig/userfieldconfig/userfieldconfig-list.md) method ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentId":81,"fields":{"documentType":"A","field7097":"Test Field"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.userfield.document.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentId":81,"fields":{"documentType":"A","field7097":"Test Field"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.userfield.document.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'catalog.userfield.document.update',
            {
                documentId: 81,
                fields: {
                    documentType: 'A',
                    field7097: 'Test Field',
                }
            }
        );

        const result = response.getData().result;
        console.log(result.document);
    }
    catch (error)
    {
        console.error('Error updating document user fields:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.userfield.document.update',
                [
                    'documentId' => 81,
                    'fields' => [
                        'documentType' => 'A',
                        'field7097' => 'Test Field',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result['document']);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating document user fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.userfield.document.update',
        {
            documentId: 81,
            fields: {
                documentType: 'A',
                field7097: 'Test Field'
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.userfield.document.update',
        [
            'documentId' => 81,
            'fields' => [
                'documentType' => 'A',
                'field7097' => 'Test Field',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result['result']['document']);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "document": {
            "documentId": 81,
            "documentType": "A",
            "field7097": "Test Field"
        }
    },
    "time": {
        "start": 1774341924,
        "finish": 1774341924.459929,
        "duration": 0.4599289894104004,
        "processing": 0,
        "date_start": "2026-03-24T11:45:24+01:00",
        "date_finish": "2026-03-24T11:45:24+01:00",
        "operating_reset_at": 1774342524,
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
|| **document**
[`catalog_userfield_document`](../data-types.md#catalog_userfield_document) | Object with updated values of the document's custom fields ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

HTTP Code: **400**

```json
{
    "error": "0",
    "error_description": "The specified document does not exist"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | The specified document does not exist | Document with the specified `documentId` not found ||
|| `0` | Access Denied | Insufficient permissions to modify the document of the selected type ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-userfield-document-list.md)
- [{#T}](../enum/catalog-enum-get-store-document-types.md)
- [{#T}](../../crm/universal/userfieldconfig/userfieldconfig/userfieldconfig-list.md)