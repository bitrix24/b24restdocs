# Update Document crm.documentgenerator.document.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for document generator documents

The method `crm.documentgenerator.document.update` updates an existing document.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../data-types.md) | Document identifier ||
|| **values**
[`object`](../../data-types.md) | An object containing the field values of the document.

Format:

```json
{
    "field_1": "value_1",
    "field_2": "value_2"
}
```

where:
- `field_n` — document field code
- `value_n` — field value

The list of available fields depends on the template and document.

You can view the fields using the method [crm.documentgenerator.document.getfields](./crm-document-generator-document-get-fields.md) ||
|| **stampsEnabled**
[`integer`](../../data-types.md) | Include stamp and signature:
- `1` — include
- `0` — do not include

Default is `1` ||
|#

{% note info "Method Feature" %}

`pdfUrl` and `imageUrl` may be absent immediately after the update, as conversion is performed asynchronously. Check the final status using [crm.documentgenerator.document.get](./crm-document-generator-document-get.md)

{% endnote %}

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

Example of updating a document where:
- Document `id` — `61`
- New document number — `2026-002`
- Stamp inclusion is enabled

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":61,"values":{"DocumentNumber":"2026-002"},"stampsEnabled":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.document.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":61,"values":{"DocumentNumber":"2026-002"},"stampsEnabled":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.document.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.documentgenerator.document.update',
    		{
    			id: 61,
    			values: {
    				DocumentNumber: '2026-002',
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
                'crm.documentgenerator.document.update',
                [
                    'id' => 61,
                    'values' => [
                        'DocumentNumber' => '2026-002',
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
        echo 'Error updating document: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.document.update',
        {
            id: 61,
            values: {
                DocumentNumber: '2026-002',
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
        'crm.documentgenerator.document.update',
        [
            'id' => 61,
            'values' => [
                'DocumentNumber' => '2026-002',
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
            "publicUrl": null,
            "title": "Demo Product Implementation 2026-001",
            "number": "2026-002",
            "id": "61",
            "createTime": "2026-03-20T14:42:39+01:00",
            "createdBy": null,
            "updateTime": "2026-03-20T14:42:38+01:00",
            "updatedBy": 577,
            "stampsEnabled": true,
            "isTransformationError": false,
            "values": {
                "productsTableVariant": "",
                "_creationMethod": "rest",
                "stampsEnabled": true,
                "DocumentNumber": "2026-002"
            },
            "templateId": "39",
            "pullTag": "TRANSFORMDOCUMENT61",
            "imageUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.getImage&SITE_ID=s1&id=61",
            "pdfUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.getPdf&SITE_ID=s1&id=61",
            "emailDiskFile": 5611,
            "entityId": "101",
            "entityTypeId": "2",
            "downloadUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.download.json?..."
        }
    },
    "time": {
        "start": 1774006958,
        "finish": 1774006960.122164,
        "duration": 2.122164011001587,
        "processing": 2,
        "date_start": "2026-03-20T14:42:38+01:00",
        "date_finish": "2026-03-20T14:42:40+01:00",
        "operating_reset_at": 1774007558,
        "operating": 1.9965031147003174
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
[`object`](../../data-types.md) | Data of the updated document. Structure is described in the [`document`](#document) type ||
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
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the user who created the document ||
|| **updatedBy**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the user who updated the document ||
|| **changeStampsEnabled**
[`boolean`](../../data-types.md) | Whether the stamp and signature inclusion can be changed ||
|| **changeStampsDisabledReason**
[`string`](../../data-types.md) | Reason why the stamp and signature inclusion cannot be changed ||
|| **changeQrCodeEnabled**
[`boolean`](../../data-types.md) | Whether the QR code can be enabled or disabled ||
|| **qrCodeEnabled**
[`boolean`](../../data-types.md) | Current state of the QR code ||
|| **changeQrCodeDisabledReason**
[`string`](../../data-types.md) | Reason why the QR code cannot be changed ||
|| **products**
[`object`](../../data-types.md) | Summary information about the document's products (`currencyId`, `totalSum`, `totalRows`) ||
|| **stampsEnabled**
[`boolean`](../../data-types.md) | Stamp and signature inclusion status ||
|| **downloadUrl**
[`string`](../../data-types.md) | Link to download the document ||
|| **downloadUrlMachine**
[`string`](../../data-types.md) | Link to download the document for machine access ||
|| **publicUrl**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Public link to the document ||
|| **isTransformationError**
[`boolean`](../../data-types.md) | Indicates if there was a conversion error with the document ||
|| **transformationErrorMessage**
[`string`](../../data-types.md) | Conversion error message, if `isTransformationError = true` ||
|| **transformationErrorCode**
[`string`](../../data-types.md) | Conversion error code, if `isTransformationError = true` ||
|| **templateId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Template identifier ||
|| **pullTag**
[`string`](../../data-types.md) | Document transformation event tag ||
|| **emailDiskFile**
[`integer`](../../data-types.md) | Identifier of the file in Drive for sending via email ||
|| **entityTypeId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the CRM object type ||
|| **entityId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the CRM object ||
|| **values**
[`object`](../../data-types.md) | Field values of the document after the update ||
|| **imageUrl**
[`string`](../../data-types.md) | Link to the document image, if already created ||
|| **pdfUrl**
[`string`](../../data-types.md) | Link to the PDF file of the document, if already created ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "DOCGEN_ACCESS_ERROR",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | Bitrix\\DocumentGenerator\\Document constructor must be public | Required parameter `id` not provided ||
|| `DOCGEN_ACCESS_ERROR` | Access denied | No access to the document ||
|| `0` | Document not found | Document with the specified `id` not found or unavailable ||
|| `0` | Document not found | Document does not belong to the CRM module ||
|| `0` | You do not have permissions to modify this document | Insufficient permissions to modify the document ||
|| `0` | Document generator module is not installed | `documentgenerator` module is unavailable ||
|| `0` | Cannot update unsaved document | Attempt to update an unsaved document ||
|| `0` | DOCUMENT_FILE_NOT_PROCESSABLE_ERROR | Template file is corrupted or has an invalid structure ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-document-add.md)
- [{#T}](./crm-document-generator-document-get.md)
- [{#T}](./crm-document-generator-document-get-fields.md)
- [{#T}](./crm-document-generator-document-list.md)
- [{#T}](./crm-document-generator-document-delete.md)
- [{#T}](../templates/crm-document-generator-template-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)