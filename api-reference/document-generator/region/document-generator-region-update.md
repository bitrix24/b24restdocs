# Update Region documentgenerator.region.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.region.update` updates a user-defined region by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the user-defined region.

You can obtain the identifier after [creating a region](./document-generator-region-add.md) or by using the [method to get the list of regions](./document-generator-region-list.md) ||
|| **fields***
[`object`](../../data-types.md) | Parameters for updating [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | New name of the region ||
|| **languageId**
[`string`](../../data-types.md) | Identifier of the region's language, for example `de` ||
|| **formatDate**
[`string`](../../data-types.md) | Date format ||
|| **formatDatetime**
[`string`](../../data-types.md) | Date and time format ||
|| **formatName**
[`string`](../../data-types.md) | Full name template, for example `#LAST_NAME# #NAME# #SECOND_NAME#`.

Possible modifiers include:

- `#TITLE#` — salutation
- `#NAME#` — first name
- `#LAST_NAME#` — last name
- `#SECOND_NAME#` — middle name
- `#NAME_SHORT#` — first letter of the first name with a dot
- `#LAST_NAME_SHORT#` — first letter of the last name with a dot
- `#SECOND_NAME_SHORT#` — first letter of the middle name with a dot

{% note tip "User Documentation" %}

- [What are modifiers in document templates](https://helpdesk.bitrix24.com/open/25799327/)

{% endnote %}

||
|| **phrases**
[`object`](../../data-types.md) | Set of region phrases in the format `code: text`.

The keys of the object are strings and can be defined arbitrarily depending on the template. The values of the keys are also strings.

To understand which fields are used in the document template, retrieve their list using the [documentgenerator.template.getfields](../templates/document-generator-template-get-fields.md) method. This will help you determine which template values require their own language phrases.

Examples:

- `TAX_INCLUDED`: `Tax included in the price`
- `TAX_NOT_INCLUDED`: `VAT not included in the price`
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "id": 1,
      "fields": {
        "title": "Germany (User-defined)",
        "formatDate": "YYYY-MM-DD",
        "phrases": {
          "TAX_INCLUDED": "Tax included in the price"
        }
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.region.update
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "id": 1,
      "fields": {
        "title": "Germany (User-defined)",
        "formatDate": "YYYY-MM-DD",
        "phrases": {
          "TAX_INCLUDED": "Tax included in the price"
        }
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.region.update
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.region.update',
          {
              id: 1,
              fields: {
                  title: 'Germany (User-defined)',
                  formatDate: 'YYYY-MM-DD',
                  phrases: {
                      TAX_INCLUDED: 'Tax included in the price'
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
          'documentgenerator.region.update',
          [
              'id' => 1,
              'fields' => [
                  'title' => 'Germany (User-defined)',
                  'formatDate' => 'YYYY-MM-DD',
                  'phrases' => [
                      'TAX_INCLUDED' => 'Tax included in the price',
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
      'documentgenerator.region.update',
      {
          id: 1,
          fields: {
              title: 'Germany (User-defined)',
              formatDate: 'YYYY-MM-DD',
              phrases: {
                  TAX_INCLUDED: 'Tax included in the price'
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
      'documentgenerator.region.update',
      [
          'id' => 1,
          'fields' => [
              'title' => 'Germany (User-defined)',
              'formatDate' => 'YYYY-MM-DD',
              'phrases' => [
                  'TAX_INCLUDED' => 'Tax included in the price',
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
            "title": "Germany (User-defined)",
            "languageId": "de",
            "formatDate": "YYYY-MM-DD",
            "formatDatetime": "DD.MM.YYYY HH:MI:SS",
            "formatName": "#LAST_NAME# #NAME# #SECOND_NAME#",
            "phrases": {
                "TAX_INCLUDED": "Tax included in the price",
                "TAX_NOT_INCLUDED": "VAT not included in the price"
            }
        }
    },
    "time": {
        "start": 1774505427,
        "finish": 1774505427.737558,
        "duration": 0.7375578880310059,
        "processing": 0,
        "date_start": "2026-03-26T09:10:27+01:00",
        "date_finish": "2026-03-26T09:10:27+01:00",
        "operating_reset_at": 1774506027,
        "operating": 0.13835597038269043
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
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
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
[`string`](../../data-types.md) | Identifier of the user-defined region ||
|| **title**
[`string`](../../data-types.md) | Name of the region ||
|| **languageId**
[`string`](../../data-types.md) | Identifier of the region's language ||
|| **formatDate**
[`string`](../../data-types.md) | Date format ||
|| **formatDatetime**
[`string`](../../data-types.md) | Date and time format ||
|| **formatName**
[`string`](../../data-types.md) | Full name template ||
|| **phrases**
[`object`](../../data-types.md) | Set of phrases in the format `code: text`, where keys and values are strings.

Examples of keys: `TAX_INCLUDED`, `TAX_NOT_INCLUDED`

You can retrieve the list of template fields using the [documentgenerator.template.getfields](../templates/document-generator-template-get-fields.md) method.
||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Region not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {id} | Required parameter `id` is missing ||
|| `400` | `0` | Region not found | The parameter `id` refers to a non-existent region ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` is missing ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to modify document generator templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-region-add.md)
- [{#T}](./document-generator-region-get.md)
- [{#T}](./document-generator-region-list.md)
- [{#T}](./document-generator-region-delete.md)