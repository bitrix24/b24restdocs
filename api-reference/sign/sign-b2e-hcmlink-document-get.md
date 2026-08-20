# Retrieve Signed HCM Link Document sign.b2e.hcmlink.document.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sign.b2e`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The `sign.b2e.hcmlink.document.get` method returns data of a signed e-Signature for HR document linked to HCM Link.

The method works only in the context of [application](../../settings/app-installation/index.md) authorization. The method is available if the HCM Link integration is available in Bitrix24.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Signing participant identifier.

You can retrieve the identifier from the `data.id` field of the [OnSignHcmLinkB2eDocumentSigned](./events/on-sign-hcm-link-b2e-document-signed.md) event ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":3942,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sign.b2e.hcmlink.document.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type HcmLinkDocument = {
      company: string
      employee: string
      document: {
        date: ISODate
        name: string
        fileUrl: string
        fileName: string
        uid: string | null
        documentUid: string | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<HcmLinkDocument>({
        method: 'sign.b2e.hcmlink.document.get',
        params: {
          id: 3942,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.company, result.employee, result.document.fileName)
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
      async function getHcmLinkDocument() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sign.b2e.hcmlink.document.get',
            params: {
              id: 3942,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.company, result.employee, result.document.fileName)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getHcmLinkDocument)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sign.b2e.hcmlink.document.get',
                [
                    'id' => 3942
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'sign.b2e.hcmlink.document.get',
        {
            id: 3942
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sign.b2e.hcmlink.document.get',
        [
            'id' => 3942
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ```

- Go

    ```go
    // client and ctx are already created. See the Go SDK section.
    res, err := client.Core().Call(ctx, "sign.b2e.hcmlink.document.get", b24.Params{
    	"id": 3942,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sign.b2e.hcmlink.document.get: %w", err)
    }

    var item struct {
    	Company string `json:"company"`
    	Employee string `json:"employee"`
    	Document struct {
    		FileName string `json:"fileName"`
    	} `json:"document"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("response parsing: %w", err)
    }
    fmt.Println(item.Company, item.Employee, item.Document.FileName)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "company": "acme-hr",
        "employee": "EMP-001",
        "document": {
            "date": "2026-08-07T10:15:30+02:00",
            "name": "Employment Contract",
            "fileUrl": "https://test.bitrix24.com/rest/download.json?auth=***&token=sign.b2e...",
            "fileName": "Employment Contract.pdf",
            "uid": "TD-2026-001",
            "documentUid": "R-LK-50JI-3AAK-WS1A"
        }
    },
    "time": {
        "start": 1786086930.123,
        "finish": 1786086930.456,
        "duration": 0.333,
        "processing": 0.111,
        "date_start": "2026-08-07T10:15:30+02:00",
        "date_finish": "2026-08-07T10:15:30+02:00",
        "operating_reset_at": 1786087530,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Signed document information [(detailed description)](#result) ||
|| **time**
[`time`](../data-types.md#time) | Request execution time information ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **company**
[`string`](../data-types.md) | HCM Link company code ||
|| **employee**
[`string`](../data-types.md) | HCM Link employee code ||
|| **document**
[`object`](../data-types.md) | Signed document data [(detailed description)](#document) ||
|#

#### Object document {#document}

#|
|| **Name**
`type` | **Description** ||
|| **date**
[`string`](../data-types.md) | Document creation date in ISO 8601 format ||
|| **name**
[`string`](../data-types.md) | Document name ||
|| **fileUrl**
[`string`](../data-types.md) | Link for downloading the signed file ||
|| **fileName**
[`string`](../data-types.md) | Signed file name ||
|| **uid**
[`string`](../data-types.md) | Document identifier from HCM Link. Can return `null` if the document field is not linked to the signed document ||
|| **documentUid**
[`string`](../data-types.md) | Unique e-Signature for HR document identifier. Can return `null` ||
|#

## Error Handling

HTTP status: **200**, **403**

```json
{
    "error": "SIGN_HCMLINK_DOCUMENT_NOT_FOUND",
    "error_description": "Document not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **When It Occurs** ||
|| `403` | `ACCESS_DENIED` | Access denied | Insufficient permissions ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | The method was not called in the application context ||
|| `200` | Empty value | Module humanresources is not available | The `humanresources` module is not available ||
|| `200` | `SIGN_HCMLINK_DOCUMENT_NOT_FOUND` | Document not found | The document was not found or is not linked to HCM Link ||
|| `200` | `SIGN_HCMLINK_DOCUMENT_NOT_LINKED` | No employee or company linked to the document | No employee or HCM Link company is linked to the document ||
|| `200` | `SIGN_HCMLINK_NO_SIGNED_FILE_EXIST` | Signed file for this document does not exist | The signed document file was not found ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-document-send.md)
- [{#T}](./sign-b2e-document-get.md)
- [{#T}](./events/on-sign-hcm-link-b2e-document-signed.md)
- [{#T}](./hcm-link/index.md)
