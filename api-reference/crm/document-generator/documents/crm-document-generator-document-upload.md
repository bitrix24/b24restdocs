# Upload Document crm.documentgenerator.document.upload

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view document generator documents

The method `crm.documentgenerator.document.upload` uploads a prepared document and attaches it to a CRM object.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields**^*^
[`object`](../../../data-types.md) | Object with document upload parameters [(more details)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**^*^
[`integer`](../../../data-types.md) | Identifier of the [CRM object type](../../data-types.md#object_type) ||
|| **entityId**^*^
[`integer`](../../../data-types.md) | Identifier of the CRM object to which the document is attached ||
|| **fileContent**^*^
[`string`](../../../data-types.md) | Content of the DOCX file in base64 format.

More details: [How to upload files](../../../files/how-to-upload-files.md) ||
|| **region**^*^
[`string`](../../../data-types.md) | Template region code, for example, `ru`. The list of regions can be retrieved using [documentgenerator.region.list](../../../document-generator/region/document-generator-region-list.md) ||
|| **title**^*^
[`string`](../../../data-types.md) | Document name ||
|| **number**^*^
[`string`](../../../data-types.md) | Document number ||
|| **pdfContent**
[`string`](../../../data-types.md) | Content of the PDF file in base64 format.

More details: [How to upload files](../../../files/how-to-upload-files.md) ||
|| **imageContent**
[`string`](../../../data-types.md) | Content of the image in base64 format.

More details: [How to upload files](../../../files/how-to-upload-files.md) ||
|#

Required fields must contain values other than an empty string or the string `0`. The `entityId` value must be greater than zero.

{% note info "Method specifics" %}

`pdfUrl` and `imageUrl` may be absent immediately after upload, as conversion is performed asynchronously. To check the result, use [crm.documentgenerator.document.get](./crm-document-generator-document-get.md)

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of uploading a document where:
- `entityTypeId` — `2` (deal)
- `entityId` — `101`
- `region` — `ru`
- document title — Product demonstration
- document number — `2026-001`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"entityTypeId":2,"entityId":101,"fileContent":"**base64_docx_content**","region":"ru","title":"Product demonstration","number":"2026-001"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.document.upload
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"entityTypeId":2,"entityId":101,"fileContent":"**base64_docx_content**","region":"ru","title":"Product demonstration","number":"2026-001"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.document.upload
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentUploadResult = {
      document: {
        id: string
        title: string
        number: string
        createTime: ISODate
        updateTime: ISODate
        createdBy: string
        updatedBy: string | null
        changeStampsEnabled: boolean
        changeStampsDisabledReason: string
        changeQrCodeEnabled: boolean
        qrCodeEnabled: boolean
        changeQrCodeDisabledReason: string
        products: {
          currencyId: string | null
          totalSum: string | number | null
          totalRows: string | number | null
        }
        stampsEnabled: boolean
        isTransformationError: boolean
        downloadUrl: string
        downloadUrlMachine: string
        publicUrl: string | null
        templateId: string
        pullTag: string
        emailDiskFile: number
        entityTypeId: string
        entityId: string
        values: Record<string, unknown> | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<DocumentUploadResult>({
        method: 'crm.documentgenerator.document.upload',
        params: {
          fields: {
            entityTypeId: 2,
            entityId: 101,
            fileContent: '**base64_docx_content**',
            region: 'de',
            title: 'Product demonstration',
            number: '2026-001',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.document.id, result.document.title, result.document.downloadUrl)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function uploadDocument() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.documentgenerator.document.upload',
            params: {
              fields: {
                entityTypeId: 2,
                entityId: 101,
                fileContent: '**base64_docx_content**',
                region: 'de',
                title: 'Product demonstration',
                number: '2026-001',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.document.id, result.document.title, result.document.downloadUrl)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', uploadDocument)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.documentgenerator.document.upload(
            fields={
                "entityTypeId": 2,
                "entityId": 101,
                "fileContent": "**base64_docx_content**",
                "region": "de",
                "title": "Product demonstration",
                "number": "2026-001",
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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
                        'title' => 'Product demonstration',
                        'number' => '2026-001',
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
                title: 'Product demonstration',
                number: '2026-001',
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
                'title' => 'Product demonstration',
                'number' => '2026-001',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.documentgenerator.document.upload", b24.Params{
    	"fields": b24.Params{
    		"entityTypeId": 2,
    		"entityId":     101,
    		"fileContent":  "**base64_docx_content**",
    		"region":       "ru",
    		"title":        "Product demonstration",
    		"number":       "2026-001",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.documentgenerator.document.upload: %w", err)
    }

    // The method wraps the response in an object with the "document" key.
    raw, ok := b24.Unwrap(res.Result, "document")
    if !ok {
    	return fmt.Errorf("no document key in the response")
    }

    var item struct {
    	ChangeStampsEnabled        bool   `json:"changeStampsEnabled"`
    	ChangeStampsDisabledReason string `json:"changeStampsDisabledReason"`
    	ChangeQrCodeEnabled        bool   `json:"changeQrCodeEnabled"`
    	QrCodeEnabled              bool   `json:"qrCodeEnabled"`
    	ChangeQrCodeDisabledReason string `json:"changeQrCodeDisabledReason"`
    	DownloadUrl                string `json:"downloadUrl"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ChangeStampsEnabled, item.ChangeStampsDisabledReason)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "document": {
            "changeStampsEnabled": false,
            "changeStampsDisabledReason": "The template is missing seals and signatures",
            "changeQrCodeEnabled": false,
            "qrCodeEnabled": false,
            "changeQrCodeDisabledReason": "The template is missing a QR code",
            "products": {
                "currencyId": "UAH",
                "totalSum": "0.00",
                "totalRows": 0
            },
            "downloadUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.download&SITE_ID=s1&id=63",
            "publicUrl": null,
            "title": "Product demonstration",
            "number": "2026-001",
            "id": "63",
            "createTime": "2026-03-20T17:08:24+03:00",
            "createdBy": "577",
            "updateTime": "2026-03-20T17:08:24+03:00",
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
        "date_start": "2026-03-20T17:08:24+03:00",
        "date_finish": "2026-03-20T17:08:25+03:00",
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
[`object`](../../../data-types.md) | Root object of the response. Contains the structure [`result`](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Type {#result}

#|
|| **Name**
`type` | **Description** ||
|| **document**
[`object`](../../../data-types.md) | Data of the uploaded document. Structure described in type [`document`](#document) ||
|#

#### Document Type {#document}

#|
|| **Name**
`type` | **Description** ||
|| **id**
```integer | string``` | Document identifier ||
|| **title**
[`string`](../../../data-types.md) | Document name ||
|| **number**
[`string`](../../../data-types.md) | Document number ||
|| **createTime**
[`datetime`](../../../data-types.md) | Create date ||
|| **updateTime**
[`datetime`](../../../data-types.md) | Update date ||
|| **createdBy**
```integer | string``` | Identifier of the user who created the document ||
|| **updatedBy**
```integer | string | null``` | Identifier of the user who updated the document ||
|| **changeStampsEnabled**
[`boolean`](../../../data-types.md) | Can the stamp and signature inclusion be changed ||
|| **changeStampsDisabledReason**
[`string`](../../../data-types.md) | Reason why the stamp and signature inclusion cannot be changed ||
|| **changeQrCodeEnabled**
[`boolean`](../../../data-types.md) | Can the QR code be enabled or disabled ||
|| **qrCodeEnabled**
[`boolean`](../../../data-types.md) | Current state of the QR code ||
|| **changeQrCodeDisabledReason**
[`string`](../../../data-types.md) | Reason why the QR code cannot be changed ||
|| **products**
[`object`](../../../data-types.md) | Summary information about the document's products: `currencyId` — currency code, a string or `null`; `totalSum` — total amount, a number, numeric string, or `null`; `totalRows` — row count, a number, numeric string, or `null` ||
|| **stampsEnabled**
[`boolean`](../../../data-types.md) | Stamp and signature inclusion flag ||
|| **downloadUrl**
[`string`](../../../data-types.md) | Link to download the document ||
|| **downloadUrlMachine**
[`string`](../../../data-types.md) | Link to download the document for machine access ||
|| **publicUrl**
```string | null``` | Public link to the document ||
|| **isTransformationError**
[`boolean`](../../../data-types.md) | Flag indicating a document conversion error ||
|| **transformationErrorMessage**
[`string`](../../../data-types.md) | Text of the conversion error if `isTransformationError = true` ||
|| **transformationErrorCode**
[`string`](../../../data-types.md) | Code of the conversion error if `isTransformationError = true` ||
|| **templateId**
```integer | string``` | Identifier of the template ||
|| **pullTag**
[`string`](../../../data-types.md) | Event tag for document transformation ||
|| **emailDiskFile**
[`integer`](../../../data-types.md) | Identifier of the file in Drive for sending via email ||
|| **entityTypeId**
```integer | string``` | Identifier of the CRM object type ||
|| **entityId**
```integer | string``` | Identifier of the CRM object ||
|| **values**
```object | null``` | Field values of the document ||
|| **imageUrl**
[`string`](../../../data-types.md) | Link to the document image, if already created ||
|| **pdfUrl**
[`string`](../../../data-types.md) | Link to the PDF file of the document, if already created ||
|| **imageUrlMachine**
[`string`](../../../data-types.md) | Link to the document image for machine access ||
|| **pdfUrlMachine**
[`string`](../../../data-types.md) | Link to the PDF document for machine access ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Empty required fields: entityTypeId, fileContent, region, entityId, title, number"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `400` | `0` | Empty required fields: entityTypeId, fileContent, region, entityId, title, number | Required fields are missing from the empty `fields` object ||
|| `400` | `0` | Empty required fields: fileContent | The required field `fields.fileContent` was not provided ||
|| `400` | `0` | Wrong "entityTypeId" field value | An invalid `fields.entityTypeId` was provided ||
|| `400` | `0` | No provider for entityTypeId | No suitable CRM provider found for `fields.entityTypeId` ||
|| `400` | `0` | Wrong "entityId" field value | An invalid `fields.entityId` was provided ||
|| `400` | `0` | Could not save file | Failed to save the uploaded file ||
|| `400` | `0` | Module crm is not installed | The `crm` module is unavailable during DG processing ||
|| `400` | `0` | Wrong provider ... | Incorrect document provider defined ||
|| `400` | `0` | Application not found | Failed to determine the current REST application ||
|| `400` | `0` | Error generating file for template | Failed to create a service file for the hidden template ||
|| `400` | `0` | Error getting template | Failed to get or create a hidden template for upload ||
|| `400` | `0` | You do not have permissions to view documents | Insufficient permissions to view documents ||
|| `400` | `0` | Module documentgenerator is not installed | The `documentgenerator` module is not available ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-document-add.md)
- [{#T}](./crm-document-generator-document-update.md)
- [{#T}](./crm-document-generator-document-get.md)
- [{#T}](./crm-document-generator-document-list.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)
