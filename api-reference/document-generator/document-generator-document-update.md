# Update Existing Document documentgenerator.document.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view documents and to modify this document

The method `documentgenerator.document.update` updates a document with new field values.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Document identifier.

You can obtain the document identifier after [creating a document](./document-generator-document-add.md) or by using the [get document list method](./document-generator-document-list.md) ||
|| **values**
[`object`](../data-types.md) | Field values of the document in the format `{"FieldCode":"Value"}` ||
|| **stampsEnabled**
[`integer`](../data-types.md) | Stamp and signature mode:
- `1` — enable
- `0` — disable

Default is `1` ||
|| **fields**
[`object`](../data-types.md) | Description of how to interpret and format values from `values` [(detailed description)](#fields).

The key of the `fields` object must match the field code from the template.

If you are only passing plain text, this parameter can be omitted.

Example structure of `fields`:

```json
{
    "CurrentDate": {
        "TYPE": "DATE",
        "FORMAT": {
            "format": "d.m.Y"
        },
        "TITLE": "Contract Date"
    }
}
```
||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE**
[`string`](../data-types.md) | Field type.

Types for which formatting can be specified:
- `DATE` — date or date-time
- `NAME` — full name
||
|| **FORMAT**
[`object`](../data-types.md) | Formatting parameters for the field type.

`FORMAT` is not fixed to one value; it should be chosen according to your template and output requirements.

For `DATE`, the `format` value is specified in the format of date modifiers of the document generator. Example: `{"format":"d.m.Y"}`

For `NAME`:
- `format` specifies the output template for name parts, e.g., `#NAME# #LAST_NAME#`
- `case` specifies the case

{% note tip "User Documentation" %}

- [What are modifiers in document templates](https://helpdesk.bitrix24.com/open/25799327/)

{% endnote %}
||
|| **PROVIDER**
[`string`](../data-types.md) | Provider class ||
|| **TITLE**
[`string`](../data-types.md) | Field title ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":51,"values":{"DocumentNumber":"DC-2026-001","CurrentDate":"2026-03-20T00:00:00+02:00","ClientName":"Müller GmbH","ClientPhone":"+49 30 12345678","Total":"130000","Comment":"Payment within 3 business days after signing","UserName":"Klaus Weber"},"stampsEnabled":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.document.update
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":51,"values":{"DocumentNumber":"DC-2026-001","CurrentDate":"2026-03-20T00:00:00+02:00","ClientName":"Müller GmbH","ClientPhone":"+49 30 12345678","Total":"130000","Comment":"Payment within 3 business days after signing","UserName":"Klaus Weber"},"stampsEnabled":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.document.update
  ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentUpdateResult = {
      document: {
        id: string
        title: string
        number: string
        templateId: string
        provider: string
        value: string
        values: Record<string, unknown>
        stampsEnabled: boolean
        downloadUrl: string
        pdfUrl: string
        imageUrl: string
        printUrl: string
        downloadUrlMachine: string
        pdfUrlMachine: string
        imageUrlMachine: string
        printUrlMachine: string
        publicUrl: string
        publicUrlView: { time: ISODate | null }
        isTransformationError: boolean
        pullTag: string
        emailDiskFile: number
        createTime: ISODate | null
        updateTime: ISODate | null
        createdBy: string
        updatedBy: number | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<DocumentUpdateResult>({
        method: 'documentgenerator.document.update',
        params: {
          id: 51,
          values: {
            DocumentNumber: 'DG-2026-001',
            CurrentDate: '2026-03-20T00:00:00+03:00',
            ClientName: 'Romashka LLC',
            ClientPhone: '+49 30 12345678',
            Total: '130000',
            Comment: 'Payment within 3 business days after signing',
            UserName: 'Ivan Petrov',
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
      async function updateDocument() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'documentgenerator.document.update',
            params: {
              id: 51,
              values: {
                DocumentNumber: 'DG-2026-001',
                CurrentDate: '2026-03-20T00:00:00+03:00',
                ClientName: 'Romashka LLC',
                ClientPhone: '+49 30 12345678',
                Total: '130000',
                Comment: 'Payment within 3 business days after signing',
                UserName: 'Ivan Petrov',
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

      document.addEventListener('DOMContentLoaded', updateDocument)
    </script>
    ```

- Python

  ```python
  from b24pysdk.errors import BitrixAPIError, BitrixSDKException

  try:
      bitrix_response = client.documentgenerator.document.update(
          bitrix_id=51,
          values={
              "DocumentNumber": "DG-2026-001",
              "CurrentDate": "2026-03-20T00:00:00+03:00",
              "ClientName": "Müller GmbH",
              "ClientPhone": "+49 30 12345678",
              "Total": "130000",
              "Comment": "Payment within 3 business days after signing",
              "UserName": "Klaus Weber",
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
      $response = $b24Service->core->call(
          'documentgenerator.document.update',
          [
              'id' => 51,
              'values' => [
                  'DocumentNumber' => 'DC-2026-001',
                  'CurrentDate' => '2026-03-20T00:00:00+02:00',
                  'ClientName' => 'Müller GmbH',
                  'ClientPhone' => '+49 30 12345678',
                  'Total' => '130000',
                  'Comment' => 'Payment within 3 business days after signing',
                  'UserName' => 'Klaus Weber',
              ],
              'stampsEnabled' => 1,
          ]
      );

      $result = $response->getResponseData()->getResult();
      print_r($result);
  } catch (Throwable $e) {
      echo $e->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'documentgenerator.document.update',
      {
          id: 51,
          values: {
              DocumentNumber: 'DC-2026-001',
              CurrentDate: '2026-03-20T00:00:00+02:00',
              ClientName: 'Müller GmbH',
              ClientPhone: '+49 30 12345678',
              Total: '130000',
              Comment: 'Payment within 3 business days after signing',
              UserName: 'Klaus Weber'
          },
          stampsEnabled: 1
      },
      function(result)
      {
          if (result.error())
          {
              console.error(result.error());
          }
          else
          {
              console.log(result.data());
          }
      }
  );
  ```

- PHP CRest

  ```php
  require_once('crest.php');

  $result = CRest::call(
      'documentgenerator.document.update',
      [
          'id' => 51,
          'values' => [
              'DocumentNumber' => 'DC-2026-001',
              'CurrentDate' => '2026-03-20T00:00:00+02:00',
              'ClientName' => 'Müller GmbH',
              'ClientPhone' => '+49 30 12345678',
              'Total' => '130000',
              'Comment' => 'Payment within 3 business days after signing',
              'UserName' => 'Klaus Weber',
          ],
          'stampsEnabled' => 1,
      ]
  );

  print_r($result);
  ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "documentgenerator.document.update", b24.Params{
    	"id": 51,
    	"values": b24.Params{
    		"DocumentNumber": "DC-2026-001",
    		"CurrentDate":    "2026-03-20T00:00:00+02:00",
    		"ClientName":     "Müller GmbH",
    		"ClientPhone":    "+49 30 12345678",
    		"Total":          "130000",
    		"Comment":        "Payment within 3 business days after signing",
    		"UserName":       "Klaus Weber",
    	},
    	"stampsEnabled": 1,
    })
    if err != nil {
    	return fmt.Errorf("documentgenerator.document.update: %w", err)
    }

    // The method wraps the response in an object with the "document" key.
    raw, ok := b24.Unwrap(res.Result, "document")
    if !ok {
    	return fmt.Errorf("no document key in the response")
    }

    var item struct {
    	DownloadUrl string `json:"downloadUrl"`
    	PublicUrl   string `json:"publicUrl"`
    	Title       string `json:"title"`
    	Number      string `json:"number"`
    	ID          b24.ID `json:"id"`
    	CreateTime  string `json:"createTime"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.DownloadUrl, item.PublicUrl)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "document": {
            "downloadUrl": "/bitrix/services/main/ajax.php?action=documentgenerator.api.document.getfile&SITE_ID=s1&id=51&ts=1773844068",
            "publicUrl": "https://mysite.com/~bat5R",
            "title": "SUPPLY_CONTRACT Template 1773843147554 DC-2026-001",
            "number": "DC-2026-001",
            "id": "51",
            "createTime": "2026-03-19T15:08:20+02:00",
            "createdBy": null,
            "updateTime": "2026-03-19T15:08:19+02:00",
            "updatedBy": 503,
            "stampsEnabled": true,
            "isTransformationError": false,
            "value": "SUPPLY_CONTRACT_2026_015",
            "values": {
                "productsTableVariant": "",
                "_creationMethod": "rest",
                "stampsEnabled": true,
                "DocumentNumber": "DC-2026-001",
                "CurrentDate": "2026-03-20T00:00:00+02:00",
                "ClientName": "Müller GmbH",
                "ClientPhone": "+49 30 12345678",
                "Total": "130000",
                "Comment": "Payment within 3 business days after signing",
                "UserName": "Klaus Weber"
            },
            "publicUrlView": {
                "time": "2026-03-19T15:08:05+02:00"
            },
            "templateId": "53",
            "provider": "Bitrix\\DocumentGenerator\\DataProvider\\Rest",
            "pullTag": "TRANSFORMDOCUMENT51",
            "emailDiskFile": 5573,
            "downloadUrlMachine": "https://mysite.com/rest/documentgenerator.api.document.getfile.json?auth=1fe8bb690000071b00000844000001f7f0f107826a2f4f01a384d1dcdb624764d8ea63&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRmaWxlJlNJVEVfSUQ9czEmaWQ9NTEmdHM9MTc3Mzg0NDA2OCZfPVptQjFqSzNTNlhtMm5EbUR6OFV6YlJydExGbGtaOGpF%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRmaWxlfGRvY3VtZW50Z2VuZXJhdG9yfFlXTjBhVzl1UFdSdlkzVnRaVzUwWjJWdVpYSmhkRzl5TG1Gd2FTNWtiMk4xYldWdWRDNW5aWFJtYVd4bEpsTkpWRVZmU1VROWN6RW1hV1E5TlRFbWRITTlNVGMzTXpnME5EQTJPQ1pmUFZwdFFqRnFTek5UTmxodE1tNUViVVI2T0ZWNllsSnlkRXhHYkd0YU9HcEZ8MWZlOGJiNjkwMDAwMDcxYjAwMDAwODQ0MDAwMDAxZjdmMGYxMDc4MjZhMmY0ZjAxYTM4NGQxZGNkYjYyNDc2NGQ4ZWE2MyI%3D.Cifk0KKcqbktmPy6ARfXPn7dO71%2FP4MY5xys1%2FGF%2FBc%3D"
        }
    },
    "time": {
        "start": 1773922099,
        "finish": 1773922099.372946,
        "duration": 0.37294602394104004,
        "processing": 0,
        "date_start": "2026-03-19T15:08:19+02:00",
        "date_finish": "2026-03-19T15:08:19+02:00",
        "operating_reset_at": 1773922699,
        "operating": 0.13009309768676758
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **document**
[`object`](../data-types.md) | Data of the updated document [(detailed description)](#document) ||
|#

#### Object document {#document}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | Document identifier. In the [documentgenerator.document.add](./document-generator-document-add.md) response, it is returned as a number ||
|| **title**
[`string`](../data-types.md) | Document title ||
|| **number**
[`string`](../data-types.md) | Document number ||
|| **templateId**
[`string`](../data-types.md) | Template identifier ||
|| **provider**
[`string`](../data-types.md) | Data provider class ||
|| **value**
[`string`](../data-types.md) | External identifier of the object for which the document was created ||
|| **values**
[`object`](../data-types.md) | Field values of the document [(detailed description)](#document-values) ||
|| **stampsEnabled**
[`boolean`](../data-types.md) | Indicator of enabled stamps and signatures ||
|| **downloadUrl**
[`string`](../data-types.md) | Link to download DOCX ||
|| **downloadUrlMachine**
[`string`](../data-types.md) | Link to download DOCX for the application ||
|| **pdfUrl**
[`string`](../data-types.md) | Link to download PDF.

The field is returned only when the PDF has already been generated. Conversion is asynchronous, so the field is usually absent immediately after the document is created or updated ||
|| **pdfUrlMachine**
[`string`](../data-types.md) | Link to download PDF for the application. Returned together with `pdfUrl` ||
|| **printUrl**
[`string`](../data-types.md) | Link to view or print the PDF. Returned together with `pdfUrl` ||
|| **printUrlMachine**
[`string`](../data-types.md) | Link to view or print the PDF for the application. Returned together with `pdfUrl` ||
|| **imageUrl**
[`string`](../data-types.md) | Link to download the document image.

The field is returned only when the image has already been generated ||
|| **imageUrlMachine**
[`string`](../data-types.md) | Link to download the image for the application. Returned together with `imageUrl` ||
|| **publicUrl**
[`string`](../data-types.md) | Public link to the document. If the link is not enabled, `null`.

You can enable the link using the [documentgenerator.document.enablepublicurl](./document-generator-document-enable-public-url.md) method ||
|| **publicUrlView**
[`object`](../data-types.md) | Data on viewing the public link [(detailed description)](#public-url-view).

The field is returned only if the public link is enabled ||
|| **isTransformationError**
[`boolean`](../data-types.md) | Indicator of an error when converting the document to PDF and image ||
|| **transformationErrorCode**
[`string`](../data-types.md) | Conversion error code. The field is returned only if `isTransformationError` = `true` ||
|| **transformationErrorMessage**
[`string`](../data-types.md) | Conversion error text. The field is returned only if `isTransformationError` = `true` ||
|| **transformationCancelReason**
[`string`](../data-types.md) | Reason the conversion was canceled. The field is returned only if the conversion was canceled ||
|| **pullTag**
[`string`](../data-types.md) | Tag for subscribing to conversion status updates ||
|| **emailDiskFile**
[`integer`](../data-types.md) | Identifier of the file on Drive for sending by e-mail: the PDF file if it has been generated, otherwise the DOCX file. If the file is not stored on Drive, `0` ||
|| **createTime**
[`datetime`](../data-types.md) | Document creation time ||
|| **createdBy**
[`string`](../data-types.md) | Identifier of the document author. In the [documentgenerator.document.add](./document-generator-document-add.md) response, it is returned as a number.

In the [documentgenerator.document.update](./document-generator-document-update.md) response, it is `null`, and `createTime` contains the modification time. The [documentgenerator.document.get](./document-generator-document-get.md) method returns the actual values ||
|| **updateTime**
[`datetime`](../data-types.md) | Time of the last document modification ||
|| **updatedBy**
[`string`](../data-types.md) | Identifier of the user who last modified the document. If the document has not been modified, `null`. In the [documentgenerator.document.update](./document-generator-document-update.md) response, it is returned as a number ||
|#

#### Object values {#document-values}

#|
|| **Name**
`type` | **Description** ||
|| **productsTableVariant**
[`string`](../data-types.md) | Variant of the product table display ||
|| **_creationMethod**
[`string`](../data-types.md) | Method of document creation ||
|| **stampsEnabled**
[`boolean`](../data-types.md) | Indicator of enabled stamps and signatures ||
|| **<field_code>**
[`string`](../data-types.md) | Value of the field from the template by its code ||
|#

#### Object publicUrlView {#public-url-view}

#|
|| **Name**
`type` | **Description** ||
|| **time**
[`datetime`](../data-types.md) | Time of the last view of the public link. If the link has not been opened yet, `null` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Document was not found."
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {id} | Required parameter `id` not provided ||
|| `400` | `0` | Document was not found | Document with the specified `id` not found ||
|| `400` | `0` | You do not have permissions to view documents | No permission to view documents. The method checks this permission first, then the permission to modify the document ||
|| `400` | `0` | You do not have permissions to modify this document | The user can view documents but has no permission to modify the document with the specified `id` ||
|| `400` | `DOCGEN_ACCESS_ERROR` | Access denied | No access to the document with the specified `id` ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-document-add.md)
- [{#T}](./document-generator-document-get.md)
- [{#T}](./document-generator-document-list.md)
- [{#T}](./document-generator-document-delete.md)
- [{#T}](./document-generator-document-enable-public-url.md)
- [{#T}](./document-generator-document-get-fields.md)