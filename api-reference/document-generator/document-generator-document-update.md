# Update Existing Document documentgenerator.document.update

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the method: user with permission to modify documents

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
    -d '{"id":51,"values":{"DocumentNumber":"DC-2026-001","CurrentDate":"2026-03-20T00:00:00+02:00","ClientName":"Ltd. Chamomile","ClientPhone":"+1 999 123-45-67","Total":"130000","Comment":"Payment within 3 business days after signing","UserName":"John Smith"},"stampsEnabled":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.document.update
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":51,"values":{"DocumentNumber":"DC-2026-001","CurrentDate":"2026-03-20T00:00:00+02:00","ClientName":"Ltd. Chamomile","ClientPhone":"+1 999 123-45-67","Total":"130000","Comment":"Payment within 3 business days after signing","UserName":"John Smith"},"stampsEnabled":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.document.update
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.document.update',
          {
              id: 51,
              values: {
                  DocumentNumber: 'DC-2026-001',
                  CurrentDate: '2026-03-20T00:00:00+02:00',
                  ClientName: 'Ltd. Chamomile',
                  ClientPhone: '+1 999 123-45-67',
                  Total: '130000',
                  Comment: 'Payment within 3 business days after signing',
                  UserName: 'John Smith'
              },
              stampsEnabled: 1
          }
      );

      const result = response.getData().result;
      console.log(result);
  }
  catch (error)
  {
      console.error(error);
  }
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
                  'ClientName' => 'Ltd. Chamomile',
                  'ClientPhone' => '+1 999 123-45-67',
                  'Total' => '130000',
                  'Comment' => 'Payment within 3 business days after signing',
                  'UserName' => 'John Smith',
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
              ClientName: 'Ltd. Chamomile',
              ClientPhone: '+1 999 123-45-67',
              Total: '130000',
              Comment: 'Payment within 3 business days after signing',
              UserName: 'John Smith'
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
              'ClientName' => 'Ltd. Chamomile',
              'ClientPhone' => '+1 999 123-45-67',
              'Total' => '130000',
              'Comment' => 'Payment within 3 business days after signing',
              'UserName' => 'John Smith',
          ],
          'stampsEnabled' => 1,
      ]
  );

  print_r($result);
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
            "createTime": "2026-03-18T17:27:48+02:00",
            "createdBy": "503",
            "updateTime": "2026-03-18T17:27:48+02:00",
            "updatedBy": null,
            "stampsEnabled": true,
            "isTransformationError": false,
            "value": "SUPPLY_CONTRACT_2026_015",
            "values": {
                "productsTableVariant": "",
                "_creationMethod": "rest",
                "stampsEnabled": true,
                "CurrentDate": "2026-03-20T00:00:00+02:00",
                "ClientName": "Ltd. Chamomile",
                "ClientPhone": "+1 999 123-45-67",
                "Total": "130000",
                "Comment": "Payment within 5 business days after signing",
                "UserName": "John Smith"
            },
            "publicUrlView": {
                "time": "2026-03-19T15:08:05+02:00"
            },
            "templateId": "53",
            "provider": "Bitrix\\DocumentGenerator\\DataProvider\\Rest",
            "pullTag": "TRANSFORMDOCUMENT51",
            "imageUrl": "/bitrix/services/main/ajax.php?action=documentgenerator.api.document.getimage&SITE_ID=s1&id=51&ts=1773844068",
            "pdfUrl": "/bitrix/services/main/ajax.php?action=documentgenerator.api.document.getpdf&SITE_ID=s1&id=51&ts=1773844068",
            "printUrl": "/bitrix/services/main/ajax.php?action=documentgenerator.api.document.showpdf&SITE_ID=s1&id=51&print=y&ts=1773844068",
            "emailDiskFile": 5573,
            "downloadUrlMachine": "https://mysite.com/rest/documentgenerator.api.document.getfile.json?auth=1fe8bb690000071b00000844000001f7f0f107826a2f4f01a384d1dcdb624764d8ea63&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRmaWxlJlNJVEVfSUQ9czEmaWQ9NTEmdHM9MTc3Mzg0NDA2OCZfPVptQjFqSzNTNlhtMm5EbUR6OFV6YlJydExGbGtaOGpF%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRmaWxlfGRvY3VtZW50Z2VuZXJhdG9yfFlXTjBhVzl1UFdSdlkzVnRaVzUwWjJWdVpYSmhkRzl5TG1Gd2FTNWtiMk4xYldWdWRDNW5aWFJtYVd4bEpsTkpWRVZmU1VROWN6RW1hV1E5TlRFbWRITTlNVGMzTXpnME5EQTJPQ1pmUFZwdFFqRnFTek5UTmxodE1tNUViVVI2T0ZWNllsSnlkRXhHYkd0YU9HcEZ8MWZlOGJiNjkwMDAwMDcxYjAwMDAwODQ0MDAwMDAxZjdmMGYxMDc4MjZhMmY0ZjAxYTM4NGQxZGNkYjYyNDc2NGQ4ZWE2MyI%3D.Cifk0KKcqbktmPy6ARfXPn7dO71%2FP4MY5xys1%2FGF%2FBc%3D",
            "imageUrlMachine": "https://mysite.com/rest/documentgenerator.api.document.getimage.json?auth=1fe8bb690000071b00000844000001f7f0f107826a2f4f01a384d1dcdb624764d8ea63&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRpbWFnZSZTSVRFX0lEPXMxJmlkPTUxJnRzPTE3NzM4NDQwNjgmXz1GOTV5aUg2SnN2UTlrZjRnQnNBN2Z6NXpxVjIxSzB1Zg%3D%3D%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRpbWFnZXxkb2N1bWVudGdlbmVyYXRvcnxZV04wYVc5dVBXUnZZM1Z0Wlc1MFoyVnVaWEpoZEc5eUxtRndhUzVrYjJOMWJXVnVkQzVuWlhScGJXRm5aU1pUU1ZSRlgwbEVQWE14Sm1sa1BUVXhKblJ6UFRFM056TTRORFF3TmpnbVh6MUdPVFY1YVVnMlNuTjJVVGxyWmpSblFuTkJOMlo2TlhweFZqSXhTekIxWmc9PXwxZmU4YmI2OTAwMDAwNzFiMDAwMDA4NDQwMDAwMDFmN2YwZjEwNzgyNmEyZjRmMDFhMzg0ZDFkY2RiNjI0NzY0ZDhlYTYzIg%3D%3D.r2wwtKof5kdIyUr8xek%2B8XThT4RG48ZMcLABVjsa0Zk%3D",
            "pdfUrlMachine": "https://mysite.com/rest/documentgenerator.api.document.getpdf.json?auth=1fe8bb690000071b00000844000001f7f0f107826a2f4f01a384d1dcdb624764d8ea63&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRwZGYmU0lURV9JRD1zMSZpZD01MSZ0cz0xNzczODQ0MDY4Jl89UnUxUGRvM2RWV0xXTEhXZDhhNW9VeThDTlVsWlJTd3M%3D%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRwZGZ8ZG9jdW1lbnRnZW5lcmF0b3J8WVdOMGFXOXVQV1J2WTNWdFpXNTBaMlZ1WlhKaGRHOXlMbUZ3YVM1a2IyTjFiV1Z1ZEM1blpYUndaR1ltVTBsVVJWOUpSRDF6TVNacFpEMDFNU1owY3oweE56Y3pPRFEwTURZNEpsODlVblV4VUdSdk0yUldWMHhYVEVoWFpEaGhOVzlWZVRoRFRsVnNXbEpUZDNNPXwxZmU4YmI2OTAwMDAwNzFiMDAwMDA4NDQwMDAwMDFmN2YwZjEwNzgyNmEyZjRmMDFhMzg0ZDFkY2RiNjI0NzY0ZDhlYTYzIg%3D%3D.u%2Bkuio%2FsYEU93FKvdsODqjyEAZ7WpP0M9We%2BurZvjUk%3D",
            "printUrlMachine": "https://mysite.com/rest/documentgenerator.api.document.showpdf.json?auth=1fe8bb690000071b00000844000001f7f0f107826a2f4f01a384d1dcdb624764d8ea63&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5zaG93cGRmJlNJVEVfSUQ9czEmaWQ9NTEmcHJpbnQ9eSZ0cz0xNzczODQ0MDY4Jl89cXFmSUhjODk1MTI4RXRWQXE3Nk5UNEdzN1dHWTlZdjU%3D%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5zaG93cGRmfGRvY3VtZW50Z2VuZXJhdG9yfFlXTjBhVzl1UFdSdlkzVnRaVzUwWjJWdVpYSmhkRzl5TG1Gd2FTNWtiMk4xYldWdWRDNXphRzkzY0dSbUpsTkpWRVZmU1VROWN6RW1hV1E5TlRFbWNISnBiblE5ZVNaMGN6MHhOemN6T0RRME1EWTRKbDg5Y1hGbVNVaGpPRGsxTVRJNFJYUldRWEUzTms1VU5FZHpOMWRIV1RsWmRqVT18MWZlOGJiNjkwMDAwMDcxYjAwMDAwODQ0MDAwMDAxZjdmMGYxMDc4MjZhMmY0ZjAxYTM4NGQxZGNkYjYyNDc2NGQ4ZWE2MyI%3D.s%2F4zPE4fPTMvZImi5pumUXn5s0oB5nwVbw3rFIM34eo%3D"
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
[`string`](../data-types.md) | Document identifier ||
|| **title**
[`string`](../data-types.md) | Document title ||
|| **number**
[`string`](../data-types.md) | Document number ||
|| **templateId**
[`string`](../data-types.md) | Template identifier ||
|| **provider**
[`string`](../data-types.md) | Data provider class ||
|| **value**
[`string`](../data-types.md) | External identifier of the object ||
|| **fileId**
[`integer`](../data-types.md) | Identifier of the DOCX file on Drive ||
|| **imageId**
[`integer`](../data-types.md) | Identifier of the preview image on Drive ||
|| **pdfId**
[`integer`](../data-types.md) | Identifier of the PDF file on Drive ||
|| **values**
[`object`](../data-types.md) | Field values of the document [(detailed description)](#document-values) ||
|| **stampsEnabled**
[`boolean`](../data-types.md) | Indicator of enabled stamps and signatures ||
|| **downloadUrl**
[`string`](../data-types.md) | Link to download DOCX for the user ||
|| **pdfUrl**
[`string`](../data-types.md) | Link to download PDF for the user ||
|| **imageUrl**
[`string`](../data-types.md) | Link to preview image for the user ||
|| **printUrl**
[`string`](../data-types.md) | Link to print PDF for the user ||
|| **downloadUrlMachine**
[`string`](../data-types.md) | Link to download DOCX for the application ||
|| **pdfUrlMachine**
[`string`](../data-types.md) | Link to download PDF for the application ||
|| **imageUrlMachine**
[`string`](../data-types.md) | Link to preview image for the application ||
|| **printUrlMachine**
[`string`](../data-types.md) | Link to print PDF for the application ||
|| **publicUrl**
[`string`](../data-types.md) | Public link to the document, if enabled ||
|| **publicUrlView**
[`object`](../data-types.md) | Parameters of the public link [(detailed description)](#public-url-view) ||
|| **isTransformationError**
[`boolean`](../data-types.md) | Indicator of document conversion error ||
|| **pullTag**
[`string`](../data-types.md) | Tag for subscribing to conversion status updates ||
|| **emailDiskFile**
[`integer`](../data-types.md) | Identifier of the file on Drive for sending by e-mail ||
|| **createTime**
[`datetime`](../data-types.md) | Document creation time ||
|| **updateTime**
[`datetime`](../data-types.md) | Document last update time ||
|| **createdBy**
[`string`](../data-types.md) | Identifier of the document author ||
|| **updatedBy**
[`integer`](../data-types.md) | Identifier of the user who updated the document ||
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
[`datetime`](../data-types.md) | Date and time until which the public link is valid ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Document not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\DocumentGenerator\Document constructor must be public | Required parameter `id` not provided ||
|| `400` | `0` | Document not found | Document with the specified `id` not found ||
|| `400` | `0` | You do not have permissions to view documents | Insufficient rights to modify the document ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-document-add.md)
- [{#T}](./document-generator-document-get.md)
- [{#T}](./document-generator-document-list.md)
- [{#T}](./document-generator-document-delete.md)
- [{#T}](./document-generator-document-enable-public-url.md)
- [{#T}](./document-generator-document-get-fields.md)