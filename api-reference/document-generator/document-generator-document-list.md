# Get the list of documents documentgenerator.document.list

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the method: user with permission to view documents

The method `documentgenerator.document.list` returns a list of documents based on the filter.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../data-types.md) | The array contains a [list of fields](#list-fields) to return.

By default `["*"]` ||
|| **order**
[`object`](../data-types.md) | An object for sorting documents in the format `{"field_1":"value_1", ... "field_N":"value_N"}`.

The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending

For `field_N`, use fields from the [fields table](#list-fields). ||
|| **filter**
[`object`](../data-types.md) | An object for filtering documents in the format `{"field_1":"value_1", ... "field_N":"value_N"}`.

For `field_N`, use fields from the [fields table](#list-fields).

You can add a prefix to the keys `field_n` to specify the filter operation. 
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character in the filter value should not be passed. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` character should be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

||
|| **start**
[`integer`](../data-types.md) | This parameter is used to control pagination.

The page size of results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N — 1) * 50`, where `N` — the desired page number ||
|#

### Fields for select, order, filter {#list-fields}

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
[`string`](../data-types.md) | Provider class ||
|| **value**
[`string`](../data-types.md) | External identifier of the object ||
|| **fileId**
[`integer`](../data-types.md) | Identifier of the document's DOCX file ||
|| **imageId**
[`integer`](../data-types.md) | Identifier of the document's image file ||
|| **pdfId**
[`integer`](../data-types.md) | Identifier of the document's PDF file ||
|| **createTime**
[`datetime`](../data-types.md) | Document creation time ||
|| **updateTime**
[`datetime`](../data-types.md) | Document update time ||
|| **values**
[`object`](../data-types.md) | Document field values ||
|| **createdBy**
[`integer`](../data-types.md) | Identifier of the user who created the document ||
|| **updatedBy**
[`integer`](../data-types.md) | Identifier of the user who updated the document ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "select": [
        "id",
        "title",
        "number",
        "templateId",
        "provider",
        "value",
        "fileId",
        "imageId",
        "pdfId",
        "createTime",
        "updateTime",
        "createdBy"
      ],
      "order": {
        "updateTime": "desc",
        "id": "desc"
      },
      "filter": {
        ">=createTime": "2026-03-18T00:00:00+02:00",
        "%title": "DG-2026"
      },
      "start": 0
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.document.list
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "select": [
        "id",
        "title",
        "number",
        "templateId",
        "provider",
        "value",
        "fileId",
        "imageId",
        "pdfId",
        "createTime",
        "updateTime",
        "createdBy"
      ],
      "order": {
        "updateTime": "desc",
        "id": "desc"
      },
      "filter": {
        ">=createTime": "2026-03-18T00:00:00+02:00",
        "%title": "DG-2026"
      },
      "start": 0,
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.document.list
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.document.list',
          {
              select: [
                  'id',
                  'title',
                  'number',
                  'templateId',
                  'provider',
                  'value',
                  'fileId',
                  'imageId',
                  'pdfId',
                  'createTime',
                  'updateTime',
                  'createdBy'
              ],
              order: {
                  updateTime: 'desc',
                  id: 'desc'
              },
              filter: {
                  '>=createTime': '2026-03-18T00:00:00+02:00',
                  '%title': 'DG-2026'
              },
              start: 0
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
          'documentgenerator.document.list',
          [
              'select' => [
                  'id',
                  'title',
                  'number',
                  'templateId',
                  'provider',
                  'value',
                  'fileId',
                  'imageId',
                  'pdfId',
                  'createTime',
                  'updateTime',
                  'createdBy',
              ],
              'order' => [
                  'updateTime' => 'desc',
                  'id' => 'desc',
              ],
              'filter' => [
                  '>=createTime' => '2026-03-18T00:00:00+02:00',
                  '%title' => 'DG-2026',
              ],
              'start' => 0,
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
      'documentgenerator.document.list',
      {
          select: [
              'id',
              'title',
              'number',
              'templateId',
              'provider',
              'value',
              'fileId',
              'imageId',
              'pdfId',
              'createTime',
              'updateTime',
              'createdBy'
          ],
          order: {
              updateTime: 'desc',
              id: 'desc'
          },
          filter: {
              '>=createTime': '2026-03-18T00:00:00+02:00',
              '%title': 'DG-2026'
          },
          start: 0
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
      'documentgenerator.document.list',
      [
          'select' => [
              'id',
              'title',
              'number',
              'templateId',
              'provider',
              'value',
              'fileId',
              'imageId',
              'pdfId',
              'createTime',
              'updateTime',
              'createdBy',
          ],
          'order' => [
              'updateTime' => 'desc',
              'id' => 'desc',
          ],
          'filter' => [
              '>=createTime' => '2026-03-18T00:00:00+02:00',
              '%title' => 'DG-2026',
          ],
          'start' => 0,
      ]
  );

  print_r($result);
  ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "documents": [
            {
                "id": "51",
                "title": "SUPPLY_CONTRACT Template 1773843147554 DG-2026-001",
                "number": "DG-2026-001",
                "templateId": "53",
                "provider": "bitrix\\documentgenerator\\dataprovider\\rest",
                "value": "SUPPLY_CONTRACT_2026_015",
                "fileId": "241",
                "imageId": "243",
                "pdfId": "245",
                "createTime": "2026-03-18T17:27:48+02:00",
                "updateTime": "2026-03-18T17:27:48+02:00",
                "createdBy": "503",
                "downloadUrl": "https://mysite.com/bitrix/services/main/ajax.php?action=documentgenerator.api.document.getfile&SITE_ID=s1&id=51",
                "pdfUrl": "https://mysite.com/bitrix/services/main/ajax.php?action=documentgenerator.api.document.getpdf&SITE_ID=s1&id=51",
                "imageUrl": "https://mysite.com/bitrix/services/main/ajax.php?action=documentgenerator.api.document.getimage&SITE_ID=s1&id=51",
                "values": null,
                "stampsEnabled": false,
                "downloadUrlMachine": "https://mysite.com/rest/documentgenerator.api.document.getfile.json?auth=63bfbb690000071b00000844000001f7f0f107a3f045d88e8327666879f4b04885d7af&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRmaWxlJlNJVEVfSUQ9czEmaWQ9NTEmXz1WMXA5WU1YMkRSbUJraDA1cmhjVVRIZXFkRE5EWmpLcA%3D%3D%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRmaWxlfGRvY3VtZW50Z2VuZXJhdG9yfFlXTjBhVzl1UFdSdlkzVnRaVzUwWjJWdVpYSmhkRzl5TG1Gd2FTNWtiMk4xYldWdWRDNW5aWFJtYVd4bEpsTkpWRVZmU1VROWN6RW1hV1E5TlRFbVh6MVdNWEE1V1UxWU1rUlNiVUpyYURBMWNtaGpWVlJJWlhGa1JFNUVXbXBMY0E9PXw2M2JmYmI2OTAwMDAwNzFiMDAwMDA4NDQwMDAwMDFmN2YwZjEwN2EzZjA0NWQ4OGU4MzI3NjY2ODc5ZjRiMDQ4ODVkN2FmIg%3D%3D.b2USzpTXZIDIUEgZjOXB4hDphKJjQY5spzTOdimZvss%3D",
                "pdfUrlMachine": "https://mysite.com/rest/documentgenerator.api.document.getpdf.json?auth=63bfbb690000071b00000844000001f7f0f107a3f045d88e8327666879f4b04885d7af&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRwZGYmU0lURV9JRD1zMSZpZD01MSZfPUgyc0IwUlpMa1BueVpvV29rajVHUnFHUWU1T0cwQ2Z1%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRwZGZ8ZG9jdW1lbnRnZW5lcmF0b3J8WVdOMGFXOXVQV1J2WTNWdFpXNTBaMlZ1WlhKaGRHOXlMbUZ3YVM1a2IyTjFiV1Z1ZEM1blpYUndaR1ltVTBsVVJWOUpSRDF6TVNacFpEMDFNU1pmUFVneWMwSXdVbHBNYTFCdWVWcHZWMjlyYWpWSFVuRkhVV1UxVDBjd1EyWjF8NjNiZmJiNjkwMDAwMDcxYjAwMDAwODQ0MDAwMDAxZjdmMGYxMDdhM2YwNDVkODhlODMyNzY2Njg3OWY0YjA0ODg1ZDdhZiI%3D.m0Ng5a%2BitODVrxQonwPkRt9L8dr2Jx9fbxnY%2BoZzAe4%3D",
                "imageUrlMachine": "https://mysite.com/rest/documentgenerator.api.document.getimage.json?auth=63bfbb690000071b00000844000001f7f0f107a3f045d88e8327666879f4b04885d7af&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRpbWFnZSZTSVRFX0lEPXMxJmlkPTUxJl89RGJHM3pFUTlPTmhYNVlrWUc3NEx6MTVUYUdzdlVkUGk%3D%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS5kb2N1bWVudC5nZXRpbWFnZXxkb2N1bWVudGdlbmVyYXRvcnxZV04wYVc5dVBXUnZZM1Z0Wlc1MFoyVnVaWEpoZEc5eUxtRndhUzVrYjJOMWJXVnVkQzVuWlhScGJXRm5aU1pUU1ZSRlgwbEVQWE14Sm1sa1BUVXhKbDg5UkdKSE0zcEZVVGxQVG1oWU5WbHJXVWMzTkV4Nk1UVlVZVWR6ZGxWa1VHaz18NjNiZmJiNjkwMDAwMDcxYjAwMDAwODQ0MDAwMDAxZjdmMGYxMDdhM2YwNDVkODhlODMyNzY2Njg3OWY0YjA0ODg1ZDdhZiI%3D.40ZdIhNinEEmMsb%2FQm%2BCseG%2BKe0ZmR6vpQhs6N6KjfQ%3D",
            },
            {
                "id": "37",
                ... // document description with id=37
            },
            {
                "id": "33",
                ... // document description with id=33
            }
        ]
    },
    "total": 3,
    "time": {
        "start": 1773908326,
        "finish": 1773908326.204212,
        "duration": 0.20421195030212402,
        "processing": 0,
        "date_start": "2026-03-19T11:18:46+02:00",
        "date_finish": "2026-03-19T11:18:46+02:00",
        "operating_reset_at": 1773908926,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **total**
[`integer`](../data-types.md) | Total number of elements based on the filter ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **documents**
[`array`](../data-types.md) | List of documents.

The composition of fields depends on `select` ||
|#

#### Document Array Element {#documents}

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
[`integer`](../data-types.md) | Identifier of the document's DOCX file ||
|| **imageId**
[`integer`](../data-types.md) | Identifier of the document's image file ||
|| **pdfId**
[`integer`](../data-types.md) | Identifier of the document's PDF file ||
|| **createTime**
[`datetime`](../data-types.md) | Document creation time ||
|| **updateTime**
[`datetime`](../data-types.md) | Time of the last update of the document ||
|| **values**
[`object`](../data-types.md) | Document field values [(detailed description)](#document-values) ||
|| **createdBy**
[`string`](../data-types.md) | Identifier of the user who created the document ||
|| **updatedBy**
[`integer`](../data-types.md) | Identifier of the user who updated the document ||
|| **downloadUrl**
[`string`](../data-types.md) | Link to download DOCX for the user ||
|| **pdfUrl**
[`string`](../data-types.md) | Link to download PDF for the user ||
|| **imageUrl**
[`string`](../data-types.md) | Link to download image for the user ||
|| **stampsEnabled**
[`boolean`](../data-types.md) | Indicator of enabled stamps and signatures ||
|| **downloadUrlMachine**
[`string`](../data-types.md) | Link to download DOCX for the application ||
|| **pdfUrlMachine**
[`string`](../data-types.md) | Link to download PDF for the application ||
|| **imageUrlMachine**
[`string`](../data-types.md) | Link to download image for the application ||
|#

#### Values Object {#document-values}

#|
|| **Name**
`type` | **Description** ||
|| **_creationMethod**
[`string`](../data-types.md) | Method of document creation ||
|| **stampsEnabled**
[`boolean`](../data-types.md) | Indicator of enabled stamps and signatures ||
|| **<field_code>**
[`string`](../data-types.md) | Value of the field from the template by its code ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "You do not have permissions to view documents"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | You do not have permissions to view documents | Insufficient rights to view the list of documents ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-document-add.md)
- [{#T}](./document-generator-document-update.md)
- [{#T}](./document-generator-document-get.md)
- [{#T}](./document-generator-document-delete.md)
- [{#T}](./document-generator-document-enable-public-url.md)
- [{#T}](./document-generator-document-get-fields.md)