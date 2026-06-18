# Get Document crm.documentgenerator.document.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" access permission for document generator documents

The method `crm.documentgenerator.document.get` returns the document data by its identifier.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../data-types.md) | Document identifier ||
|#

{% note info "Method Feature" %}

`pdfUrl` and `imageUrl` may be absent immediately after the document is created or updated, as the conversion is performed asynchronously. If the links are needed right away, repeat the request after 30-40 seconds.

{% endnote %}

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

Example of retrieving a document with identifier `61`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":61}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.document.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":61,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.document.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentGetResult = {
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
          currencyId: string
          totalSum: string
          totalRows: number
        }
        stampsEnabled: boolean
        downloadUrl: string
        downloadUrlMachine: string
        imageUrl: string
        imageUrlMachine: string
        pdfUrl: string
        pdfUrlMachine: string
        publicUrl: string | null
        isTransformationError: boolean
        templateId: string
        pullTag: string
        emailDiskFile: number
        entityTypeId: string
        entityId: string
        values: Record<string, unknown>
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<DocumentGetResult>({
        method: 'crm.documentgenerator.document.get',
        params: {
          id: 61,
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
      async function getDocument() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.documentgenerator.document.get',
            params: {
              id: 61,
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

      document.addEventListener('DOMContentLoaded', getDocument)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.documentgenerator.document.get',
                [
                    'id' => 61,
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
        echo 'Error getting document: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.document.get',
        {
            id: 61,
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
        'crm.documentgenerator.document.get',
        [
            'id' => 61,
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
            "downloadUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.download&SITE_ID=s1&id=61",
            "publicUrl": null,
            "title": "Demo Product Implementation 2026-001",
            "number": "2026-001",
            "id": "61",
            "createTime": "2026-03-20T13:51:45+01:00",
            "createdBy": "577",
            "updateTime": "2026-03-20T13:51:45+01:00",
            "updatedBy": null,
            "stampsEnabled": true,
            "isTransformationError": false,
            "values": {
                "productsTableVariant": "",
                "_creationMethod": "rest",
                "stampsEnabled": true
            },
            "templateId": "39",
            "pullTag": "TRANSFORMDOCUMENT61",
            "imageUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.getImage&SITE_ID=s1&id=61",
            "pdfUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.getPdf&SITE_ID=s1&id=61",
            "emailDiskFile": 5609,
            "entityId": "101",
            "entityTypeId": "2",
            "downloadUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.download.json?...",
            "imageUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.getImage.json?...",
            "pdfUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.getPdf.json?..."
        }
    },
    "time": {
        "start": 1774006578,
        "finish": 1774006578.790473,
        "duration": 0.7904729843139648,
        "processing": 0,
        "date_start": "2026-03-20T14:36:18+01:00",
        "date_finish": "2026-03-20T14:36:18+01:00",
        "operating_reset_at": 1774007178,
        "operating": 0.23765993118286133
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
[`object`](../../data-types.md) | Document data. The structure is described in the [`document`](#document) type ||
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
[`datetime`](../../data-types.md) | Document creation date ||
|| **updateTime**
[`datetime`](../../data-types.md) | Document update date ||
|| **createdBy**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the user who created the document ||
|| **updatedBy**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the user who updated the document ||
|| **changeStampsEnabled**
[`boolean`](../../data-types.md) | Can the stamp and signature substitution feature be changed ||
|| **changeStampsDisabledReason**
[`string`](../../data-types.md) | Reason why the stamp and signature substitution feature cannot be changed ||
|| **changeQrCodeEnabled**
[`boolean`](../../data-types.md) | Can the QR code be enabled or disabled ||
|| **qrCodeEnabled**
[`boolean`](../../data-types.md) | Current state of the QR code ||
|| **changeQrCodeDisabledReason**
[`string`](../../data-types.md) | Reason why the QR code cannot be changed ||
|| **products**
[`object`](../../data-types.md) | Summary information about the document's products (`currencyId`, `totalSum`, `totalRows`) ||
|| **stampsEnabled**
[`boolean`](../../data-types.md) | Stamp and signature substitution feature indicator ||
|| **downloadUrl**
[`string`](../../data-types.md) | Link to download the document ||
|| **downloadUrlMachine**
[`string`](../../data-types.md) | Link to download the document for machine access ||
|| **imageUrl**
[`string`](../../data-types.md) | Link to the document image. May be an empty string immediately after creation or update ||
|| **imageUrlMachine**
[`string`](../../data-types.md) | Link to the document image for machine access ||
|| **pdfUrl**
[`string`](../../data-types.md) | Link to the PDF document. May be an empty string immediately after creation or update ||
|| **pdfUrlMachine**
[`string`](../../data-types.md) | Link to the PDF document for machine access ||
|| **publicUrl**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Public link to the document ||
|| **isTransformationError**
[`boolean`](../../data-types.md) | Indicator of document conversion error ||
|| **transformationErrorMessage**
[`string`](../../data-types.md) | Text of the conversion error, if `isTransformationError = true` ||
|| **transformationErrorCode**
[`string`](../../data-types.md) | Code of the conversion error, if `isTransformationError = true` ||
|| **templateId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the document template ||
|| **pullTag**
[`string`](../../data-types.md) | Tag for the document transformation event ||
|| **emailDiskFile**
[`integer`](../../data-types.md) | Identifier of the file on Drive for sending via email ||
|| **entityTypeId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the CRM object type ||
|| **entityId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the CRM object ||
|| **values**
[`object`](../../data-types.md) | Field values of the document ||
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
|| `DOCGEN_ACCESS_ERROR` | Access denied | No access to the document ||
|| `0` | Document not found | Document with the specified `id` not found or unavailable ||
|| `Empty value` | Document not found | Document does not belong to the `crm` module ||
|| `100` | Bitrix\\DocumentGenerator\\Document constructor must be is public | Required parameter `id` not provided ||
|| `Empty value` | You do not have permissions to view documents | Insufficient rights to view document generator documents ||
|| `Empty value` | Module documentgenerator is not installed | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-document-add.md)
- [{#T}](./crm-document-generator-document-update.md)
- [{#T}](./crm-document-generator-document-get-fields.md)
- [{#T}](./crm-document-generator-document-list.md)
- [{#T}](./crm-document-generator-document-delete.md)
- [{#T}](../templates/crm-document-generator-template-get.md)