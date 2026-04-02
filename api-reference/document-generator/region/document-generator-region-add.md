# Add Region documentgenerator.region.add

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.region.add` adds a new custom region.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Region parameters [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../../data-types.md) | Name of the region ||
|| **languageId**
[`string`](../../data-types.md) | Language identifier for the region, e.g., `de` ||
|| **formatDate**
[`string`](../../data-types.md) | Date format ||
|| **formatDatetime**
[`string`](../../data-types.md) | Date and time format ||
|| **formatName**
[`string`](../../data-types.md) | Full name template, e.g., `#LAST_NAME# #NAME# #SECOND_NAME#`.

List of possible modifiers:

- `#TITLE#` — salutation
- `#NAME#` — first name
- `#LAST_NAME#` — last name
- `#SECOND_NAME#` — middle name
- `#NAME_SHORT#` — first letter of the first name with a dot
- `#LAST_NAME_SHORT#` — first letter of the last name with a dot
- `#SECOND_NAME_SHORT#` — first letter of the middle name with a dot

{% note tip "User Documentation" %}

- [What are Modifiers in Document Templates](https://helpdesk.bitrix24.com/open/25799327/)

{% endnote %}

||
|| **phrases**
[`object`](../../data-types.md) | Set of localized phrases for the region in the format `code: text` ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "fields": {
        "title": "Germany (Custom)",
        "languageId": "de",
        "formatDate": "DD.MM.YYYY",
        "formatDatetime": "DD.MM.YYYY HH:MI:SS",
        "formatName": "#LAST_NAME# #NAME# #SECOND_NAME#",
        "phrases": {
          "TAX_INCLUDED": "VAT included in price",
          "TAX_NOT_INCLUDED": "VAT not included in price"
        }
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.region.add
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "fields": {
        "title": "Germany (Custom)",
        "languageId": "de",
        "formatDate": "DD.MM.YYYY",
        "formatDatetime": "DD.MM.YYYY HH:MI:SS",
        "formatName": "#LAST_NAME# #NAME# #SECOND_NAME#",
        "phrases": {
          "TAX_INCLUDED": "VAT included in price",
          "TAX_NOT_INCLUDED": "VAT not included in price"
        }
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.region.add
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.region.add',
          {
              fields: {
                  title: 'Germany (Custom)',
                  languageId: 'de',
                  formatDate: 'DD.MM.YYYY',
                  formatDatetime: 'DD.MM.YYYY HH:MI:SS',
                  formatName: '#LAST_NAME# #NAME# #SECOND_NAME#',
                  phrases: {
                      TAX_INCLUDED: 'VAT included in price',
                      TAX_NOT_INCLUDED: 'VAT not included in price'
                  }
              }
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
          'documentgenerator.region.add',
          [
              'fields' => [
                  'title' => 'Germany (Custom)',
                  'languageId' => 'de',
                  'formatDate' => 'DD.MM.YYYY',
                  'formatDatetime' => 'DD.MM.YYYY HH:MI:SS',
                  'formatName' => '#LAST_NAME# #NAME# #SECOND_NAME#',
                  'phrases' => [
                      'TAX_INCLUDED' => 'VAT included in price',
                      'TAX_NOT_INCLUDED' => 'VAT not included in price',
                  ],
              ],
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
      'documentgenerator.region.add',
      {
          fields: {
              title: 'Germany (Custom)',
              languageId: 'de',
              formatDate: 'DD.MM.YYYY',
              formatDatetime: 'DD.MM.YYYY HH:MI:SS',
              formatName: '#LAST_NAME# #NAME# #SECOND_NAME#',
              phrases: {
                  TAX_INCLUDED: 'VAT included in price',
                  TAX_NOT_INCLUDED: 'VAT not included in price'
              }
          }
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
      'documentgenerator.region.add',
      [
          'fields' => [
              'title' => 'Germany (Custom)',
              'languageId' => 'de',
              'formatDate' => 'DD.MM.YYYY',
              'formatDatetime' => 'DD.MM.YYYY HH:MI:SS',
              'formatName' => '#LAST_NAME# #NAME# #SECOND_NAME#',
              'phrases' => [
                  'TAX_INCLUDED' => 'VAT included in price',
                  'TAX_NOT_INCLUDED' => 'VAT not included in price',
              ],
          ],
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
        "region": {
            "id": "1",
            "title": "Germany (Custom)",
            "languageId": "de",
            "formatDate": "DD.MM.YYYY",
            "formatDatetime": "DD.MM.YYYY HH:MI:SS",
            "formatName": "#LAST_NAME# #NAME# #SECOND_NAME#",
            "phrases": {
                "TAX_INCLUDED": "VAT included in price",
                "TAX_NOT_INCLUDED": "VAT not included in price"
            }
        }
    },
    "time": {
        "start": 1774431594,
        "finish": 1774431594.911013,
        "duration": 0.9110128879547119,
        "processing": 0,
        "date_start": "2026-03-25T12:39:54+01:00",
        "date_finish": "2026-03-25T12:39:54+01:00",
        "operating_reset_at": 1774432194,
        "operating": 0.23056697845458984
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **region**
[`object`](../../data-types.md) | Region data [(detailed description)](#region) ||
|#

#### Object region {#region}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the custom region ||
|| **title**
[`string`](../../data-types.md) | Name of the region ||
|| **languageId**
[`string`](../../data-types.md) | Language identifier for the region ||
|| **formatDate**
[`string`](../../data-types.md) | Date format ||
|| **formatDatetime**
[`string`](../../data-types.md) | Date and time format ||
|| **formatName**
[`string`](../../data-types.md) | Full name template ||
|| **phrases**
[`object`](../../data-types.md) | Set of phrases in the format `code: text` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {fields}"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` is missing or empty ||
|| `400` | `BX_EMPTY_REQUIRED` | Required field "TITLE" is not filled | Required field `title` is missing in the `fields` parameter ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient rights to modify document generator templates ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-region-update.md)
- [{#T}](./document-generator-region-get.md)
- [{#T}](./document-generator-region-list.md)
- [{#T}](./document-generator-region-delete.md)