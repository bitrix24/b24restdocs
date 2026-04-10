# Create a New Document Based on a Template documentgenerator.document.add

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the method: a user with document creation permissions

The method `documentgenerator.document.add` creates a new document based on a template.

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../data-types.md) | Template identifier.

You can obtain the template identifier after [creating a template](./templates/document-generator-template-add.md) or by using the [get template list](./templates/document-generator-template-list.md) method. ||
|| **providerClassName**
[`string`](../data-types.md) | Data provider class.

Defaults to `Bitrix\DocumentGenerator\DataProvider\Rest` ||
|| **value***
[`string`](../data-types.md) | External identifier of the object for which the document is being generated.

The format of `value` is defined by the integration. Use a consistent format within your application to search and filter documents.

Recommended format: `<OBJECT_TYPE>_<ID>`, where:
- `<OBJECT_TYPE>` — string code of the object type in uppercase
- `<ID>` — numeric or string identifier of the object in the external system

Examples of `value` for CRM objects:
- lead — `LEAD_123`
- contact — `CONTACT_45`
- company — `COMPANY_78`
- deal — `DEAL_901`
- estimate — `QUOTE_55`
- invoice — `INVOICE_12`

Examples of `value` for other objects:
- order — `ORDER_1024`
- supply contract — `SUPPLY_CONTRACT_2026_015`
- external system record — `ERP_DOC_A-7741`

||
|| **values**
[`object`](../data-types.md) | Field values of the document in the format `{"FieldCode":"Value"}` ||
|| **stampsEnabled**
[`integer`](../data-types.md) | Seal and signature mode:
- `1` — enable
- `0` — disable

Defaults to the value from the template ||
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

`FORMAT` is not fixed to one value; it should be chosen based on your template and output requirements.

For `DATE`, the `format` value is specified in the format of document generator date modifiers. Example: `{"format":"d.m.Y"}`

For `NAME`:
- `format` specifies the output template for name parts, e.g., `#NAME# #LAST_NAME#`
- `case` specifies the grammatical case

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

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":53,"value":"SUPPLY_CONTRACT_2026_015","values":{"DocumentNumber":"SC-2026-001","CurrentDate":"2026-03-18T00:00:00+01:00","ClientName":"ABC Corp","ClientPhone":"+1 999 123-45-67","Total":"125000","Comment":"Payment within 5 business days after signing","UserName":"John Smith"},"fields":{"CurrentDate":{"TYPE":"DATE","FORMAT":{"format":"d.m.Y"},"TITLE":"Contract Date"}},"stampsEnabled":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.document.add
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":53,"value":"SUPPLY_CONTRACT_2026_015","values":{"DocumentNumber":"SC-2026-001","CurrentDate":"2026-03-18T00:00:00+01:00","ClientName":"ABC Corp","ClientPhone":"+1 999 123-45-67","Total":"125000","Comment":"Payment within 5 business days after signing","UserName":"John Smith"},"fields":{"CurrentDate":{"TYPE":"DATE","FORMAT":{"format":"d.m.Y"},"TITLE":"Contract Date"}},"stampsEnabled":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.document.add
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.document.add',
          {
              templateId: 53,
              value: 'SUPPLY_CONTRACT_2026_015',
              values: {
                  DocumentNumber: 'SC-2026-001',
                  CurrentDate: '2026-03-18T00:00:00+01:00',
                  ClientName: 'ABC Corp',
                  ClientPhone: '+1 999 123-45-67',
                  Total: '125000',
                  Comment: 'Payment within 5 business days after signing',
                  UserName: 'John Smith'
              },
              fields: {
                  CurrentDate: {
                      TYPE: 'DATE',
                      FORMAT: {
                          format: 'd.m.Y'
                      },
                      TITLE: 'Contract Date'
                  }
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
          'documentgenerator.document.add',
          [
              'templateId' => 53,
              'value' => 'SUPPLY_CONTRACT_2026_015',
              'values' => [
                  'DocumentNumber' => 'SC-2026-001',
                  'CurrentDate' => '2026-03-18T00:00:00+01:00',
                  'ClientName' => 'ABC Corp',
                  'ClientPhone' => '+1 999 123-45-67',
                  'Total' => '125000',
                  'Comment' => 'Payment within 5 business days after signing',
                  'UserName' => 'John Smith',
              ],
              'fields' => [
                  'CurrentDate' => [
                      'TYPE' => 'DATE',
                      'FORMAT' => [
                          'format' => 'd.m.Y',
                      ],
                      'TITLE' => 'Contract Date',
                  ]
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
      'documentgenerator.document.add',
      {
          templateId: 53,
          value: 'SUPPLY_CONTRACT_2026_015',
          values: {
              DocumentNumber: 'SC-2026-001',
              CurrentDate: '2026-03-18T00:00:00+01:00',
              ClientName: 'ABC Corp',
              ClientPhone: '+1 999 123-45-67',
              Total: '125000',
              Comment: 'Payment within 5 business days after signing',
              UserName: 'John Smith'
          },
          fields: {
              CurrentDate: {
                  TYPE: 'DATE',
                  FORMAT: {
                      format: 'd.m.Y'
                  },
                  TITLE: 'Contract Date'
              }
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
      'documentgenerator.document.add',
      [
          'templateId' => 53,
          'value' => 'SUPPLY_CONTRACT_2026_015',
          'values' => [
              'DocumentNumber' => 'SC-2026-001',
              'CurrentDate' => '2026-03-18T00:00:00+01:00',
              'ClientName' => 'ABC Corp',
              'ClientPhone' => '+1 999 123-45-67',
              'Total' => '125000',
              'Comment' => 'Payment within 5 business days after signing',
              'UserName' => 'John Smith',
          ],
          'fields' => [
              'CurrentDate' => [
                  'TYPE' => 'DATE',
                  'FORMAT' => [
                      'format' => 'd.m.Y',
                  ],
                  'TITLE' => 'Contract Date',
              ]
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
            "publicUrl": null,
            "title": "SUPPLY_CONTRACT Template 1773843147554 SC-2026-001",
            "number": "SC-2026-001",
            "id": 51,
            "createTime": "2026-03-18T17:27:48+01:00",
            "createdBy": 503,
            "updateTime": "2026-03-18T17:27:48+01:00",
            "updatedBy": null,
            "stampsEnabled": true,
            "isTransformationError": false,
            "value": "SUPPLY_CONTRACT_2026_015",
            "values": {
                "productsTableVariant": "",
                "_creationMethod": "rest",
                "stampsEnabled": true,
                "DocumentNumber": "SC-2026-001",
                "CurrentDate": "2026-03-18T00:00:00+01:00",
                "ClientName": "ABC Corp",
                "ClientPhone": "+1 999 123-45-67",
                "Total": "125000",
                "Comment": "Payment within 5 business days after signing",
                "UserName": "John Smith"
            },
            "templateId": "53",
            "provider": "Bitrix\\DocumentGenerator\\DataProvider\\Rest",
            "pullTag": "TRANSFORMDOCUMENT51",
            "emailDiskFile": 5569,
            "downloadUrlMachine": "https://**put_your_bitrix24_address**/rest/documentgenerator.api.document.getfile.json?auth=a0bfba690000071b00000844000001f7f0f1075f240da39bb5ea0e42c08c5fa182f3ed&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRmaWxlJlNJVEVfSUQ9czEmaWQ9NTEmdHM9MTc3Mzg0NDA2OCZfPXZMVUFDSGMwQkY1QVpRbGQzTlNhV2ZIemNzMW5IZ1lM%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRmaWxlfGRvY3VtZW50Z2VuZXJhdG9yfFlXTjBhVzl1UFdSdlkzVnRaVzUwWjJWdVpYSmhkRzl5TG1Gd2FTNWtiMk4xYldWdWRDNW5aWFJtYVd4bEpsTkpWRVZmU1VROWN6RW1hV1E5TlRFbWRITTlNVGMzTXpnME5EQTJPQ1pmUFhaTVZVRkRTR013UWtZMVFWcFJiR1F6VGxOaFYyWkllbU56TVc1SVoxbE18YTBiZmJhNjkwMDAwMDcxYjAwMDAwODQ0MDAwMDAxZjdmMGYxMDc1ZjI0MGRhMzliYjVlYTBlNDJjMDhjNWZhMTgyZjNlZCI%3D.6lsKyiThwQT0n4UyMQfXdyS%2BnBVTG08%2FpGguggYNGLE%3D"
        }
    },
    "time": {
        "start": 1773844068,
        "finish": 1773844068.572038,
        "duration": 0.572037935256958,
        "processing": 0,
        "date_start": "2026-03-18T17:27:48+01:00",
        "date_finish": "2026-03-18T17:27:48+01:00",
        "operating_reset_at": 1773844668,
        "operating": 0.9536302089691162
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
[`object`](../data-types.md) | Data of the created document [(detailed description)](#document) ||
|#

#### Object document {#document}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Document identifier ||
|| **title**
[`string`](../data-types.md) | Document title ||
|| **number**
[`string`](../data-types.md) | Document number ||
|| **templateId**
[`integer`](../data-types.md) | Template identifier ||
|| **provider**
[`string`](../data-types.md) | Data provider class ||
|| **value**
[`string`](../data-types.md) | External identifier of the object ||
|| **values**
[`object`](../data-types.md) | Field values of the document [(detailed description)](#document-values) ||
|| **stampsEnabled**
[`boolean`](../data-types.md) | Indicator of enabled seals and signatures ||
|| **downloadUrl**
[`string`](../data-types.md) | Link to download DOCX for the user ||
|| **downloadUrlMachine**
[`string`](../data-types.md) | Link to download DOCX for the application ||
|| **publicUrl**
[`string`](../data-types.md) | Public link to the document, if enabled ||
|| **isTransformationError**
[`boolean`](../data-types.md) | Indicator of document conversion error ||
|| **pullTag**
[`string`](../data-types.md) | Tag for subscribing to conversion status updates ||
|| **emailDiskFile**
[`integer`](../data-types.md) | File identifier on Drive for sending by e-mail ||
|| **createTime**
[`datetime`](../data-types.md) | Document creation time ||
|| **updateTime**
[`datetime`](../data-types.md) | Last update time of the document ||
|| **createdBy**
[`integer`](../data-types.md) | Identifier of the document author ||
|| **updatedBy**
[`integer`](../data-types.md) | Identifier of the user who updated the document ||
|#

#### Object values {#document-values}

#|
|| **Name**
`type` | **Description** ||
|| **_creationMethod**
[`string`](../data-types.md) | Method of document creation ||
|| **stampsEnabled**
[`boolean`](../data-types.md) | Indicator of enabled seals and signatures ||
|| **<field_code>**
[`string`](../data-types.md) | Field value from the template by its code ||
|#

{% note info "" %}

File conversion to PDF is performed asynchronously. If the `pdfUrl` field is not filled immediately after creation, re-invoke [documentgenerator.document.get](./document-generator-document-get.md) to check the conversion result.

{% endnote %}

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "0",
    "error_description": "Cannot create document on deleted template"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\DocumentGenerator\Template constructor must be is public | Required parameter `templateId` is not provided ||
|| `400` | `0` | Empty required parameter "value" | Required parameter `value` is not provided ||
|| `400` | `0` | Cannot create document on deleted template | Cannot create a document based on a deleted template ||
|| `400` | `0` | Template not found | No template exists with the specified `templateId` ||
|| `400` | `0` | Cannot create document | Failed to create document ||
|| `400` | `0` | You do not have permissions to view documents | Insufficient permissions to view documents ||
|| `400` | `0` | Maximum count of documents has been reached | Document limit reached on the plan ||
|| `403` | `DOCGEN_ACCESS_ERROR` | Access denied | Insufficient permissions to create document ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-document-update.md)
- [{#T}](./document-generator-document-get.md)
- [{#T}](./document-generator-document-list.md)
- [{#T}](./document-generator-document-delete.md)
- [{#T}](./document-generator-document-enable-public-url.md)
- [{#T}](./document-generator-document-get-fields.md)