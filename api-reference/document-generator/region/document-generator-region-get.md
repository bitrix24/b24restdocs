# Get Region by ID documentgenerator.region.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.region.get` returns region data based on the identifier or code.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../data-types.md) | Identifier of the custom region or code of a pre-installed region, for example, `de`.

You can obtain the identifier after [creating a region](./document-generator-region-add.md) or by using the [get regions list method](./document-generator-region-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"1"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.region.get
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"1","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.region.get
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.region.get',
          {
              id: '1'
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
          'documentgenerator.region.get',
          [
              'id' => '1',
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
      'documentgenerator.region.get',
      {
          id: '1'
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
      'documentgenerator.region.get',
      [
          'id' => '1',
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
            "formatDate": "YYYY-MM-DD",
            "formatDatetime": "DD.MM.YYYY HH:MI:SS",
            "formatName": "#LAST_NAME# #NAME# #SECOND_NAME#",
            "phrases": {
                "TAX_INCLUDED": "Tax included in price",
                "TAX_NOT_INCLUDED": "VAT not included in price"
            }
        }
    },
    "time": {
        "start": 1774505900,
        "finish": 1774505900.746113,
        "duration": 0.7461130619049072,
        "processing": 0,
        "date_start": "2026-03-26T09:18:20+01:00",
        "date_finish": "2026-03-26T09:18:20+01:00",
        "operating_reset_at": 1774506500,
        "operating": 0
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

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **region**
[`object`](../../data-types.md) | Region data [(detailed description)](#region) ||
|#

#### Region Object {#region}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the custom region; this field may be absent for a pre-installed region ||
|| **title**
[`string`](../../data-types.md) | Name of the region ||
|| **languageId**
[`string`](../../data-types.md) | Language identifier of the region ||
|| **formatDate**
[`string`](../../data-types.md) | Date format ||
|| **formatDatetime**
[`string`](../../data-types.md) | Date and time format ||
|| **formatName**
[`string`](../../data-types.md) | Full name template ||
|| **phrases**
[`object`](../../data-types.md) | Set of phrases in the format `code: text`, where keys and values are strings.

Examples of keys: `TAX_INCLUDED`, `TAX_NOT_INCLUDED`

The list of template fields can be obtained using the [documentgenerator.template.getfields](../templates/document-generator-template-get-fields.md) method ||
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
|| `400` | `0` | Region not found | The parameter `id` specifies a non-existent region ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to modify document generator templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-region-add.md)
- [{#T}](./document-generator-region-update.md)
- [{#T}](./document-generator-region-list.md)
- [{#T}](./document-generator-region-delete.md)