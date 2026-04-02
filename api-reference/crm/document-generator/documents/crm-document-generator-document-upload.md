# Upload Document crm.documentgenerator.document.upload

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for document generator documents

The method `crm.documentgenerator.document.upload` uploads a prepared document and attaches it to a CRM entity.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields**^*^
[`object`](../../data-types.md) | Object with document upload parameters [(more details)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**^*^
[`integer`](../../data-types.md) | Identifier of the CRM entity type.

Typical values:
- `1` — lead
- `2` — deal
- `3` — contact
- `4` — company
- `7` — estimate

For SPAs, their `entityTypeId` is passed, for example, `177` ||
|| **entityId**^*^
[`integer`](../../data-types.md) | Identifier of the CRM entity to which the document is attached ||
|| **fileContent**^*^
[`string`](../../data-types.md) | Content of the DOCX file in base64 format.

More details: [How to upload files](../../../files/how-to-upload-files.md) ||
|| **region**^*^
[`string`](../../data-types.md) | Template region code, for example, `de` ||
|| **title**^*^
[`string`](../../data-types.md) | Document title ||
|| **number**^*^
[`string`](../../data-types.md) | Document number ||
|| **pdfContent**
[`string`](../../data-types.md) | Content of the PDF file in base64 format.

More details: [How to upload files](../../../files/how-to-upload-files.md) ||
|| **imageContent**
[`string`](../../data-types.md) | Content of the image in base64 format.

More details: [How to upload files](../../../files/how-to-upload-files.md) ||
|#

{% note info "Method Features" %}

`pdfUrl` and `imageUrl` may be absent immediately after upload, as conversion is performed asynchronously. To check the result, use [crm.documentgenerator.document.get](./crm-document-generator-document-get.md)

{% endnote %}

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Example of uploading a document where:
- `entityTypeId` — `2` (deal)
- `entityId` — `101`
- `region` — `de`
- document title — `Product Demonstration Implementation`
- document number — `2026-001`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"entityTypeId":2,"entityId":101,"fileContent":"**base64_docx_content**","region":"de","title":"Product Demonstration Implementation","number":"2026-001","pdfContent":"**base64_pdf_content**","imageContent":"**base64_image_content**"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.document.upload
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"entityTypeId":2,"entityId":101,"fileContent":"**base64_docx_content**","region":"de","title":"Product Demonstration Implementation","number":"2026-001","pdfContent":"**base64_pdf_content**","imageContent":"**base64_image_content**"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.document.upload
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'crm.documentgenerator.document.upload',
            {
                fields: {
                    entityTypeId: 2,
                    entityId: 101,
                    fileContent: '**base64_docx_content**',
                    region: 'de',
                    title: 'Product Demonstration Implementation',
                    number: '2026-001',
                    pdfContent: '**base64_pdf_content**',
                    imageContent: '**base64_image_content**',
                },
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
                'crm.documentgenerator.document.upload',
                [
                    'fields' => [
                        'entityTypeId' => 2,
                        'entityId' => 101,
                        'fileContent' => '**base64_docx_content**',
                        'region' => 'de',
                        'title' => 'Product Demonstration Implementation',
                        'number' => '2026-001',
                        'pdfContent' => '**base64_pdf_content**',
                        'imageContent' => '**base64_image_content**',
                    ],
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
        echo 'Error uploading document: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.document.upload',
        {
            fields: {
                entityTypeId: 2,
                entityId: 101,
                fileContent: '**base64_docx_content**',
                region: 'de',
                title: 'Product Demonstration Implementation',
                number: '2026-001',
                pdfContent: '**base64_pdf_content**',
                imageContent: '**base64_image_content**',
            },
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
        'crm.documentgenerator.document.upload',
        [
            'fields' => [
                'entityTypeId' => 2,
                'entityId' => 101,
                'fileContent' => '**base64_docx_content**',
                'region' => 'de',
                'title' => 'Product Demonstration Implementation',
                'number' => '2026-001',
                'pdfContent' => '**base64_pdf_content**',
                'imageContent' => '**base64_image_content**',
            ],
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
            "changeStampsDisabledReason": "The template does not have seals and signatures",
            "changeQrCodeEnabled": false,
            "qrCodeEnabled": false,
            "changeQrCodeDisabledReason": "The template does not have a QR code",
            "products": {
                "currencyId": "EUR",
                "totalSum": "0.00",
                "totalRows": 0
            },
            "downloadUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.download&SITE_ID=s1&id=63",
            "publicUrl": null,
            "title": "Product Demonstration Implementation",
            "number": "2026-001",
            "id": "63",
            "createTime": "2026-03-20T17:08:24+01:00",
            "createdBy": "577",
            "updateTime": "2026-03-20T17:08:24+01:00",
            "updatedBy": null,
            "stampsEnabled": false,
            "isTransformationError": false,
            "values": {
                "productsTableVariant": "",
                "_creationMethod": "rest"
            },
            "templateId": "55",
            "pullTag": "TRANSFORMDOCUMENT63",
            "emailDiskFile": 5619,
            "entityId": "101",
            "entityTypeId": "2",
            "downloadUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.download.json?auth=***&token=***"
        }
    },
    "time": {
        "start": 1774015704,
        "finish": 1774015705.08562,
        "duration": 1.0856199264526367,
        "processing": 1,
        "date_start": "2026-03-20T17:08:24+01:00",
        "date_finish": "2026-03-20T17:08:25+01:00",
        "operating_reset_at": 1774016304,
        "operating": 1.0284008979797363
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the response. Contains the structure [`result`](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **document**
[`object`](../../data-types.md) | Data of the uploaded document. Structure described in type [`document`](#document) ||
|#

#### Type document {#document}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Document identifier ||
|| **title**
[`string`](../../data-types.md) | Document title ||
|| **number**
[`string`](../../data-types.md) | Document number ||
|| **createTime**
[`datetime`](../../data-types.md) | Creation date ||
|| **updateTime**
[`datetime`](../../data-types.md) | Update date ||
|| **createdBy**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the user who created the document ||
|| **updatedBy**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the user who updated the document ||
|| **changeStampsEnabled**
[`boolean`](../../data-types.md) | Whether the seal and signature substitution feature can be changed ||
|| **changeStampsDisabledReason**
[`string`](../../data-types.md) | Reason why the seal and signature substitution feature cannot be changed ||
|| **changeQrCodeEnabled**
[`boolean`](../../data-types.md) | Whether the QR code can be enabled or disabled ||
|| **qrCodeEnabled**
[`boolean`](../../data-types.md) | Current state of the QR code ||
|| **changeQrCodeDisabledReason**
[`string`](../../data-types.md) | Reason why the QR code cannot be changed ||
|| **products**
[`object`](../../data-types.md) | Summary information about the document's products (`currencyId`, `totalSum`, `totalRows`) ||
|| **stampsEnabled**
[`boolean`](../../data-types.md) | Seal and signature substitution feature indicator ||
|| **downloadUrl**
[`string`](../../data-types.md) | Link to download the document ||
|| **downloadUrlMachine**
[`string`](../../data-types.md) | Link to download the document for machine access ||
|| **publicUrl**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Public link to the document ||
|| **isTransformationError**
[`boolean`](../../data-types.md) | Indicator of document conversion error ||
|| **transformationErrorMessage**
[`string`](../../data-types.md) | Text of the conversion error, if `isTransformationError = true` ||
|| **transformationErrorCode**
[`string`](../../data-types.md) | Code of the conversion error, if `isTransformationError = true` ||
|| **templateId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the template ||
|| **pullTag**
[`string`](../../data-types.md) | Tag of the document transformation event ||
|| **emailDiskFile**
[`integer`](../../data-types.md) | Identifier of the file in Drive for sending via email ||
|| **entityTypeId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the CRM entity type ||
|| **entityId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the CRM entity ||
|| **values**
[`object`](../../data-types.md) \| [`null`](../../data-types.md) | Values of the document fields ||
|| **imageUrl**
[`string`](../../data-types.md) | Link to the document image, if already created ||
|| **pdfUrl**
[`string`](../../data-types.md) | Link to the PDF file of the document, if already created ||
|| **imageUrlMachine**
[`string`](../../data-types.md) | Link to the document image for machine access ||
|| **pdfUrlMachine**
[`string`](../../data-types.md) | Link to the PDF document for machine access ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid value {} to match with parameter {fields}. Should be value of type array."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `100` | Invalid value {} to match with parameter {fields}. Should be value of type array. | Parameter `fields` not passed in object format ||
|| `0` | Empty required fields: entityTypeId, fileContent, region, entityId | Required fields in `fields` not provided ||
|| `0` | Empty required fields: fileContent | Required field `fields.fileContent` not provided ||
|| `0` | Wrong "entityTypeId" field value | Incorrect `fields.entityTypeId` provided ||
|| `0` | No provider for entityTypeId | No suitable CRM provider found for `fields.entityTypeId` ||
|| `0` | Wrong "entityId" field value | Incorrect `fields.entityId` provided ||
|| `0` | Missing file content | `fields.fileContent` not provided or unreadable ||
|| `0` | Could not save file | Failed to save the uploaded file ||
|| `0` | Module crm is not installed | The `crm` module is unavailable during DG processing ||
|| `0` | Wrong provider ... | Incorrect document provider defined ||
|| `0` | Application not found | Failed to determine the current REST application ||
|| `0` | Error generating file for template | Failed to create a service file for the hidden template ||
|| `0` | Error getting template | Failed to get or create a hidden template for upload ||
|| `Empty value` | You do not have permissions to view documents | Insufficient permissions to view documents ||
|| `Empty value` | Module documentgenerator is not installed | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-document-add.md)
- [{#T}](./crm-document-generator-document-get.md)
- [{#T}](./crm-document-generator-document-update.md)
- [{#T}](./crm-document-generator-document-list.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)