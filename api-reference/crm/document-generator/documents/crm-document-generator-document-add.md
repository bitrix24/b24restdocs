# Create a New Document crm.documentgenerator.document.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to view and create document generator documents and access to the CRM object used to create the document

The method `crm.documentgenerator.document.add` creates a document based on a template for a CRM object.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId**^*^
[`integer`](../../../data-types.md) | Identifier of the document template ||
|| **entityTypeId**^*^
[`integer`](../../../data-types.md) | Identifier of the [CRM object type](../../data-types.md#object_type) for which the document is created ||
|| **entityId**^*^
[`integer`](../../../data-types.md) | Identifier of the CRM object based on which the document is created ||
|| **values**
[`object`](../../../data-types.md) | Object with the values of the document fields.

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

The set of keys depends on the template and data provider. Available template fields can be retrieved using [crm.documentgenerator.template.getfields](../templates/crm-document-generator-template-get-fields.md) ||
|| **stampsEnabled**
[`integer`](../../../data-types.md) | Include stamp and signature:
- `1` — include
- `0` — do not include

Default is `0` ||
|| **fields**
[`object`](../../../data-types.md) | Additional descriptions of document fields. Available fields can be retrieved using [crm.documentgenerator.template.getfields](../templates/crm-document-generator-template-get-fields.md) and [crm.documentgenerator.document.getfields](./crm-document-generator-document-get-fields.md). The `SOURCE` and `DOCUMENT` fields are ignored ||
|#

{% note info "Method Feature" %}

`pdfUrl` and `imageUrl` may be absent immediately after creation, as conversion is performed asynchronously. To check the result, use [crm.documentgenerator.document.get](./crm-document-generator-document-get.md)

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of creating a document where:
- `templateId` — `39`
- `entityTypeId` — `2` (deal)
- `entityId` — `101`
- document number is passed via `values.DocumentNumber`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":39,"entityTypeId":2,"entityId":101,"values":{"DocumentNumber":"2026-001"},"stampsEnabled":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.document.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":39,"entityTypeId":2,"entityId":101,"values":{"DocumentNumber":"2026-001"},"stampsEnabled":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.document.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentAddResult = {
      document: {
        id: number | string
        title: string
        number: string
        createTime: ISODate
        updateTime: ISODate
        createdBy: number
        updatedBy: number | null
        changeStampsEnabled: boolean
        changeStampsDisabledReason?: string
        changeQrCodeEnabled: boolean
        qrCodeEnabled: boolean
        changeQrCodeDisabledReason?: string
        products?: {
          currencyId: string | null
          totalSum: string | number | null
          totalRows: string | number | null
        }
        stampsEnabled: boolean
        downloadUrl: string
        downloadUrlMachine: string
        publicUrl: string | null
        isTransformationError: boolean
        transformationErrorMessage?: string
        transformationErrorCode?: number | string
        templateId: number | string
        pullTag?: string
        emailDiskFile: number
        entityTypeId: number | string
        entityId: number | string
        values: Record<string, unknown>
        imageUrl?: string
        pdfUrl?: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<DocumentAddResult>({
        method: 'crm.documentgenerator.document.add',
        params: {
          templateId: 39,
          entityTypeId: 2,
          entityId: 101,
          values: {
            DocumentNumber: '2026-001',
          },
          stampsEnabled: 1,
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
      async function addDocument() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.documentgenerator.document.add',
            params: {
              templateId: 39,
              entityTypeId: 2,
              entityId: 101,
              values: {
                DocumentNumber: '2026-001',
              },
              stampsEnabled: 1,
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

      document.addEventListener('DOMContentLoaded', addDocument)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.documentgenerator.document.add(
            template_id=39,
            entity_type_id=2,
            entity_id=101,
            values={
                "DocumentNumber": "2026-001",
            },
            stamps_enabled=1,
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
                'crm.documentgenerator.document.add',
                [
                    'templateId' => 39,
                    'entityTypeId' => 2,
                    'entityId' => 101,
                    'values' => [
                        'DocumentNumber' => '2026-001',
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
            'stampsEnabled' => 1,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.documentgenerator.document.add", b24.Params{
    	"templateId":   39,
    	"entityTypeId": 2,
    	"entityId":     101,
    	"values": b24.Params{
    		"DocumentNumber": "2026-001",
    	},
    	"stampsEnabled": 1,
    })
    if err != nil {
    	return fmt.Errorf("crm.documentgenerator.document.add: %w", err)
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
            "downloadUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.download&SITE_ID=s1&id=61",
            "downloadUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.download.json?...",
            "publicUrl": null,
            "id": 61,
            "title": "Demo fulfillment of product 2026-001",
            "number": "2026-001",
            "createTime": "2026-03-20T13:51:45+03:00",
            "createdBy": 577,
            "updateTime": "2026-03-20T13:51:45+03:00",
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
        "date_start": "2026-03-20T13:51:44+03:00",
        "date_finish": "2026-03-20T13:51:45+03:00",
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
[`object`](../../../data-types.md) | Root element of the response. Returns the [`result`](#result) object ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Type {#result}

#|
|| **Name**
`type` | **Description** ||
|| **document**
[`object`](../../../data-types.md) | Data of the created document. The structure is described in the [`document`](#document) type ||
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
[`integer`](../../../data-types.md) | Identifier of the user who created the document ||
|| **updatedBy**
```integer | null``` | Identifier of the user who updated the document ||
|| **changeStampsEnabled**
[`boolean`](../../../data-types.md) | Can the stamp and signature inclusion be changed ||
|| **changeStampsDisabledReason**
[`string`](../../../data-types.md) | Reason why the stamp and signature inclusion cannot be changed. The field may be absent ||
|| **changeQrCodeEnabled**
[`boolean`](../../../data-types.md) | Can the QR code be enabled or disabled ||
|| **qrCodeEnabled**
[`boolean`](../../../data-types.md) | Current state of the QR code ||
|| **changeQrCodeDisabledReason**
[`string`](../../../data-types.md) | Reason why the QR code cannot be changed. The field may be absent ||
|| **products**
[`object`](../../../data-types.md) | Summary information about the document's products: `currencyId` — currency code, a string or `null`; `totalSum` — total amount, a number, numeric string, or `null`; `totalRows` — row count, a number, numeric string, or `null`. The field may be absent ||
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
```integer | string``` | Code of the conversion error if `isTransformationError = true` ||
|| **templateId**
```integer | string``` | Identifier of the template ||
|| **pullTag**
[`string`](../../../data-types.md) | Event tag for document transformation. The field may be absent ||
|| **emailDiskFile**
[`integer`](../../../data-types.md) | Identifier of the file in Drive for sending via email ||
|| **entityTypeId**
```integer | string``` | Identifier of the CRM object type ||
|| **entityId**
```integer | string``` | Identifier of the CRM object ||
|| **values**
[`object`](../../../data-types.md) | Values of document fields, including values passed during creation and service values ||
|| **imageUrl**
[`string`](../../../data-types.md) | Link to the document image, if already created ||
|| **pdfUrl**
[`string`](../../../data-types.md) | Link to the PDF file of the document, if already created ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "No provider for entityTypeId"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | No provider for entityTypeId | No data provider found for the provided `entityTypeId` ||
|| `400` | `0` | Empty required parameter "value" | `entityId` not provided or is empty ||
|| `400` | `0` | Cannot create document on deleted template | Cannot create a document based on a deleted template ||
|| `400` | `0` | Cannot create document | Error creating document based on the template ||
|| `400` | `DOCGEN_ACCESS_ERROR` | Access denied | No access to create the document ||
|| `400` | `DOCGEN_LIMIT_ERROR` | Maximum count of documents has been reached | Exceeded the limit of documents in the plan ||
|| `400` | `0` | Error getting next number | Failed to get the next document number from the number generator ||
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `templateId` not provided ||
|| `400` | `0` | Module documentgenerator is not installed | The `documentgenerator` module is not available ||
|| `400` | `0` | Template was not found | Template with the specified `templateId` not found ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-document-update.md)
- [{#T}](./crm-document-generator-document-get.md)
- [{#T}](./crm-document-generator-document-list.md)
- [{#T}](./crm-document-generator-document-delete.md)
- [{#T}](./crm-document-generator-document-get-fields.md)
- [{#T}](../templates/crm-document-generator-template-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)
