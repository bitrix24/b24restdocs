# Create a New Document crm.documentgenerator.document.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for document generator documents.

The method `crm.documentgenerator.document.add` creates a document based on a template for a CRM entity.

## Method Parameters

{% include [Note on Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId**^*^
[`integer`](../../data-types.md) | Identifier of the document template ||
|| **entityTypeId**^*^
[`integer`](../../data-types.md) | Identifier of the CRM entity type for which the document is created.

Typical values:
- `1` — lead
- `2` — deal
- `3` — contact
- `4` — company
- `5` — invoice (old version)
- `7` — estimate
- `31` — invoice

For Smart Processes, their `entityTypeId` is passed, for example, `177` ||
|| **entityId**^*^
[`integer`](../../data-types.md) | Identifier of the CRM entity based on which the document is created ||
|| **values**
[`object`](../../data-types.md) | Object with the values of the document fields.

Format:

```json
{
    "field_1": "value_1",
    "field_2": "value_2"
}
```

where:
- `field_n` — field code of the document
- `value_n` — field value

The set of keys depends on the specific template and data provider. Available fields can be viewed using the method [crm.documentgenerator.document.getfields](./crm-document-generator-document-get-fields.md) ||
|| **stampsEnabled**
[`integer`](../../data-types.md) | Include stamp and signature:
- `1` — include
- `0` — do not include

Default is `0` ||
|| **fields**
[`object`](../../data-types.md) | Additional descriptions of document fields for generation.

The `fields` parameter is used for more precise field configuration. In most scenarios, the `values` parameter is sufficient.

In `fields`, a field descriptor object is passed. Example:

```json
{
    "DocumentTitle": {
        "title": "Document Title",
        "value": "Demo Implementation of Product 1",
        "required": "Y",
        "default": "Demo Implementation of Product 1",
        "chain": [
            {},
            "getTitle"
        ],
        "VALUE": "Test via fields"
    }
}
```

The list of `fields` keys is not fixed and depends on the template.

How to access available parameters:
- before creating the document — [crm.documentgenerator.template.getfields](../templates/crm-document-generator-template-get-fields.md), field `templateFields`
- for the created document — [crm.documentgenerator.document.getfields](./crm-document-generator-document-get-fields.md), field `documentFields`

Service fields `SOURCE` and `DOCUMENT` are ignored.

Some computed fields may not be overridden via `fields` ||
|#

{% note info "Method Feature" %}

`pdfUrl` and `imageUrl` may be absent immediately after creation, as conversion is performed asynchronously. To check the result, use [crm.documentgenerator.document.get](./crm-document-generator-document-get.md)

{% endnote %}

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

Example of creating a document where:
- `templateId` — `39`
- `entityTypeId` — `2` (deal)
- `entityId` — `101`
- document number is passed via `values.DocumentNumber`
- for `fields.DocumentTitle`, a descriptor object is passed

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":39,"entityTypeId":2,"entityId":101,"values":{"DocumentNumber":"2026-001"},"fields":{"DocumentTitle":{"title":"Document Title","value":"Demo Implementation of Product 1","required":"Y","default":"Demo Implementation of Product 1","chain":[{},"getTitle"],"VALUE":"Test via fields"}},"stampsEnabled":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.document.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":39,"entityTypeId":2,"entityId":101,"values":{"DocumentNumber":"2026-001"},"fields":{"DocumentTitle":{"title":"Document Title","value":"Demo Implementation of Product 1","required":"Y","default":"Demo Implementation of Product 1","chain":[{},"getTitle"],"VALUE":"Test via fields"}},"stampsEnabled":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.document.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'crm.documentgenerator.document.add',
            {
                templateId: 39,
                entityTypeId: 2,
                entityId: 101,
                values: {
                    DocumentNumber: '2026-001',
                },
                fields: {
                    DocumentTitle: {
                        title: 'Document Title',
                        value: 'Demo Implementation of Product 1',
                        required: 'Y',
                        default: 'Demo Implementation of Product 1',
                        chain: [{}, 'getTitle'],
                        VALUE: 'Test via fields',
                    },
                },
                stampsEnabled: 1,
            }
        );

        const result = response.getData().result;
        console.info(result);
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
                'crm.documentgenerator.document.add',
                [
                    'templateId' => 39,
                    'entityTypeId' => 2,
                    'entityId' => 101,
                    'values' => [
                        'DocumentNumber' => '2026-001',
                    ],
                    'fields' => [
                        'DocumentTitle' => [
                            'title' => 'Document Title',
                            'value' => 'Demo Implementation of Product 1',
                            'required' => 'Y',
                            'default' => 'Demo Implementation of Product 1',
                            'chain' => [
                                [],
                                'getTitle',
                            ],
                            'VALUE' => 'Test via fields',
                        ],
                    ],
                    'stampsEnabled' => 1,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding document: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.document.add',
        {
            templateId: 39,
            entityTypeId: 2,
            entityId: 101,
            values: {
                DocumentNumber: '2026-001',
            },
            fields: {
                DocumentTitle: {
                    title: 'Document Title',
                    value: 'Demo Implementation of Product 1',
                    required: 'Y',
                    default: 'Demo Implementation of Product 1',
                    chain: [{}, 'getTitle'],
                    VALUE: 'Test via fields',
                }
            },
            stampsEnabled: 1,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.documentgenerator.document.add',
        [
            'templateId' => 39,
            'entityTypeId' => 2,
            'entityId' => 101,
            'values' => [
                'DocumentNumber' => '2026-001',
            ],
            'fields' => [
                'DocumentTitle' => [
                    'title' => 'Document Title',
                    'value' => 'Demo Implementation of Product 1',
                    'required' => 'Y',
                    'default' => 'Demo Implementation of Product 1',
                    'chain' => [
                        [],
                        'getTitle',
                    ],
                    'VALUE' => 'Test via fields',
                ],
            ],
            'stampsEnabled' => 1,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "document": {
            "changeStampsEnabled": false,
            "changeStampsDisabledReason": "No stamps and signatures in the template",
            "changeQrCodeEnabled": false,
            "qrCodeEnabled": false,
            "changeQrCodeDisabledReason": "No QR code in the template",
            "products": {
                "currencyId": "EUR",
                "totalSum": "0.00",
                "totalRows": 0
            },
            "downloadUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.download&SITE_ID=s1&id=61",
            "downloadUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.download.json?...",
            "publicUrl": null,
            "id": 61,
            "title": "Demo Implementation of Product 2026-001",
            "number": "2026-001",
            "createTime": "2026-03-20T13:51:45+01:00",
            "createdBy": 577,
            "updateTime": "2026-03-20T13:51:45+01:00",
            "updatedBy": null,
            "stampsEnabled": true,
            "isTransformationError": false,
            "values": {
                "productsTableVariant": "",
                "_creationMethod": "rest",
                "stampsEnabled": true,
                "DocumentNumber": "2026-001"
            },
            "templateId": "39",
            "pullTag": "TRANSFORMDOCUMENT61",
            "imageUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.getImage&SITE_ID=s1&id=61",
            "pdfUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.getPdf&SITE_ID=s1&id=61",
            "emailDiskFile": 5605,
            "entityId": "101",
            "entityTypeId": "2"
        }
    },
    "time": {
        "start": 1774003904,
        "finish": 1774003905.448804,
        "duration": 1.4488039016723633,
        "processing": 1,
        "date_start": "2026-03-20T13:51:44+01:00",
        "date_finish": "2026-03-20T13:51:45+01:00",
        "operating_reset_at": 1774004504,
        "operating": 0.9240179061889648
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Returns the [`result`](#result) object ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **document**
[`object`](../../data-types.md) | Data of the created document. The structure is described in the [`document`](#document) type ||
|#

#### Type document {#document}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the document ||
|| **title**
[`string`](../../data-types.md) | Title of the document ||
|| **number**
[`string`](../../data-types.md) | Document number ||
|| **createTime**
[`datetime`](../../data-types.md) | Creation date ||
|| **updateTime**
[`datetime`](../../data-types.md) | Update date ||
|| **createdBy**
[`integer`](../../data-types.md) | Identifier of the user who created the document ||
|| **updatedBy**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the user who updated the document ||
|| **changeStampsEnabled**
[`boolean`](../../data-types.md) | Can the stamp and signature inclusion be changed ||
|| **changeStampsDisabledReason**
[`string`](../../data-types.md) | Reason why the stamp and signature inclusion cannot be changed ||
|| **changeQrCodeEnabled**
[`boolean`](../../data-types.md) | Can the QR code be enabled or disabled ||
|| **qrCodeEnabled**
[`boolean`](../../data-types.md) | Current state of the QR code ||
|| **changeQrCodeDisabledReason**
[`string`](../../data-types.md) | Reason why the QR code cannot be changed ||
|| **products**
[`object`](../../data-types.md) | Summary information about the document's products (`currencyId`, `totalSum`, `totalRows`) ||
|| **stampsEnabled**
[`boolean`](../../data-types.md) | Stamp and signature inclusion flag ||
|| **downloadUrl**
[`string`](../../data-types.md) | Link to download the document ||
|| **downloadUrlMachine**
[`string`](../../data-types.md) | Link to download the document for machine access ||
|| **publicUrl**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Public link to the document ||
|| **isTransformationError**
[`boolean`](../../data-types.md) | Flag indicating a document conversion error ||
|| **transformationErrorMessage**
[`string`](../../data-types.md) | Text of the conversion error if `isTransformationError = true` ||
|| **transformationErrorCode**
[`string`](../../data-types.md) | Code of the conversion error if `isTransformationError = true` ||
|| **templateId**
[`integer`](../../data-types.md) | Identifier of the template ||
|| **pullTag**
[`string`](../../data-types.md) | Event tag for document transformation ||
|| **emailDiskFile**
[`integer`](../../data-types.md) | Identifier of the file in Drive for sending via email ||
|| **entityTypeId**
[`integer`](../../data-types.md) | Identifier of the CRM entity type ||
|| **entityId**
[`integer`](../../data-types.md) | Identifier of the CRM entity ||
|| **values**
[`object`](../../data-types.md) | Values of the document fields passed during creation ||
|| **imageUrl**
[`string`](../../data-types.md) | Link to the document image, if already created ||
|| **pdfUrl**
[`string`](../../data-types.md) | Link to the PDF file of the document, if already created ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "No provider for entityTypeId"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | No provider for entityTypeId | No data provider found for the provided `entityTypeId` ||
|| `0` | Empty required parameter "value" | `entityId` not provided or is empty ||
|| `0` | Cannot create document on deleted template | Cannot create a document based on a deleted template ||
|| `0` | Cannot create document | Error creating document based on the template ||
|| `DOCGEN_ACCESS_ERROR` | Access denied | No access to create the document ||
|| `DOCGEN_LIMIT_ERROR` | Maximum count of documents has been reached | Exceeded the limit of documents in the plan ||
|| `0` | Error getting next number | Failed to get the next document number from the number generator ||
|| `100` | Bitrix\\DocumentGenerator\\Template constructor must be public | Low-level error when calling without a valid `templateId` ||
|| `0` | Module documentgenerator is not installed | The `documentgenerator` module is not available ||
|| `0` | Template not found | Template with the specified `templateId` not found or unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-document-get.md)
- [{#T}](./crm-document-generator-document-update.md)
- [{#T}](./crm-document-generator-document-get-fields.md)
- [{#T}](./crm-document-generator-document-list.md)
- [{#T}](./crm-document-generator-document-delete.md)
- [{#T}](../templates/crm-document-generator-template-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)