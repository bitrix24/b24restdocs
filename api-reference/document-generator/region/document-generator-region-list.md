# Get the List of Regions documentgenerator.region.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.region.list` returns a list of pre-installed and custom regions.

## Method Parameters

No parameters.

## Code Examples

{% include [Example Footnote](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Accept: application/json" \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.region.list
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.region.list
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod('documentgenerator.region.list');
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
      $response = $b24Service->core->call('documentgenerator.region.list');
      $result = $response->getResponseData()->getResult();
      print_r($result);
  } catch (Throwable $e) {
      echo $e->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'documentgenerator.region.list',
      {},
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
  
  $result = CRest::call('documentgenerator.region.list');
  
  print_r($result);
  ```

{% endlist %}

## Response Handling

HTTP Status: **200**

The response may contain two types of elements:

- A pre-installed region, which typically has `code`, `title`, `languageId`
- A custom region, which may additionally have `id`, `formatDate`, `formatDatetime`, `formatName`

```json
{
    "result": {
        "regions": {
            "de": {
                "code": "de",
                "title": "Germany",
                "languageId": "de"
            },
            "uk": {
                "code": "uk",
                "title": "United Kingdom",
                "languageId": "en"
            },
            "br": {
                "code": "br",
                "title": "Brazil",
                "languageId": "br"
            },
            "sp": {
                "code": "sp",
                "title": "Spain",
                "languageId": "la"
            },
            "mx": {
                "code": "mx",
                "title": "Mexico",
                "languageId": "la"
            },
            "pl": {
                "code": "pl",
                "title": "Poland",
                "languageId": "pl"
            },
            "fr": {
                "code": "fr",
                "title": "France",
                "languageId": "fr"
            },
            "1": {
                "id": "1",
                "title": "Germany (Custom)",
                "languageId": "de",
                "formatDate": "DD.MM.YYYY",
                "formatDatetime": "DD.MM.YYYY HH:MI:SS",
                "formatName": "#LAST_NAME# #NAME# #SECOND_NAME#",
                "code": "1"
            }
        }
    },
    "time": {
        "start": 1774431603,
        "finish": 1774431603.769177,
        "duration": 0.7691769599914551,
        "processing": 0,
        "date_start": "2026-03-25T12:40:03+01:00",
        "date_finish": "2026-03-25T12:40:03+01:00",
        "operating_reset_at": 1774432203,
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

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **regions**
[`object`](../../data-types.md) | List of regions, where the object's key is the `code` of the region [(detailed description)](#regions) ||
|#

#### Object regions {#regions}

The `regions` object can contain both pre-installed and custom regions.

- For a pre-installed region, the fields `code`, `title`, `languageId` are usually available
- For a custom region, additional fields `id`, `formatDate`, `formatDatetime`, `formatName` may be returned

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the custom region; this field may be absent for a pre-installed region ||
|| **code**
[`string`](../../data-types.md) | Code of the region ||
|| **title**
[`string`](../../data-types.md) | Name of the region ||
|| **languageId**
[`string`](../../data-types.md) | Language identifier of the region ||
|| **formatDate**
[`string`](../../data-types.md) | Date format for the custom region ||
|| **formatDatetime**
[`string`](../../data-types.md) | Date and time format for the custom region ||
|| **formatName**
[`string`](../../data-types.md) | Full name template for the custom region ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "You do not have permissions to modify templates"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to modify document generator templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-region-add.md)
- [{#T}](./document-generator-region-update.md)
- [{#T}](./document-generator-region-get.md)
- [{#T}](./document-generator-region-delete.md)