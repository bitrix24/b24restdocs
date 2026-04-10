# Get Numerator by ID documentgenerator.numerator.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.numerator.get` returns the numerator data by its ID.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | The identifier of the numerator.

You can obtain the identifier after [creating a numerator](./document-generator-numerator-add.md) or by using the [method to get the list of numerators](./document-generator-numerator-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":55}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.numerator.get
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":55,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.numerator.get
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.numerator.get',
          {
              id: 55
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
          'documentgenerator.numerator.get',
          [
              'id' => 55,
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
      'documentgenerator.numerator.get',
      {
          id: 55
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
      'documentgenerator.numerator.get',
      [
          'id' => 55,
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
        "numerator": {
            "name": "REST Invoice Numerator Updated",
            "template": "INV-UPD-{NUMBER}",
            "id": "55",
            "code": null,
            "settings": {
                "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
                    "start": 2000,
                    "step": 10,
                    "length": 8,
                    "padString": "0",
                    "periodicBy": "year",
                    "timezone": "Europe/Berlin",
                    "isDirectNumeration": false
                }
            }
        }
    },
    "time": {
        "start": 1774363151,
        "finish": 1774363151.551318,
        "duration": 0.5513179302215576,
        "processing": 0,
        "date_start": "2026-03-24T17:39:11+01:00",
        "date_finish": "2026-03-24T17:39:11+01:00",
        "operating_reset_at": 1774363751,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **numerator**
[`object`](../../data-types.md) | The numerator data [(detailed description)](#result-numerator) ||
|#

#### Numerator Object {#result-numerator}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | The identifier of the numerator ||
|| **name**
[`string`](../../data-types.md) | The name of the numerator ||
|| **template**
[`string`](../../data-types.md) | The number template ||
|| **code**
[`string`](../../data-types.md) | The symbolic code of the numerator. It can be `null` ||
|| **settings**
[`object`](../../data-types.md) | The settings of the numerator generators [(detailed description)](#result-numerator-settings) ||
|#

#### Settings Object {#result-numerator-settings}

#|
|| **Name**
`type` | **Description** ||
|| **Bitrix_Main_Numerator_Generator_SequentNumberGenerator**
[`object`](../../data-types.md) | Settings for sequential numbering [(detailed description)](#result-numerator-settings-sequent) ||
|#

#### Bitrix_Main_Numerator_Generator_SequentNumberGenerator Object {#result-numerator-settings-sequent}

#|
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | The initial value of the counter ||
|| **step**
[`integer`](../../data-types.md) | The increment step of the counter ||
|| **length**
[`integer`](../../data-types.md) | The minimum length of the number ||
|| **padString**
[`string`](../../data-types.md) | The padding character on the left when `length > 0` ||
|| **periodicBy**
[`string`](../../data-types.md) | The reset period of the counter ||
|| **timezone**
[`string`](../../data-types.md) | The timezone identifier for periodic reset ||
|| **isDirectNumeration**
[`boolean`](../../data-types.md) | The indicator of direct numbering ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "DOCGEN_ACCESS_ERROR",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\Main\Numerator\Numerator constructor must be public | Required parameter `id` is missing ||
|| `400` | `100` | Could not construct parameter {numerator} | An invalid or non-existent numerator identifier was provided ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient rights to modify document generator templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-numerator-add.md)
- [{#T}](./document-generator-numerator-update.md)
- [{#T}](./document-generator-numerator-list.md)
- [{#T}](./document-generator-numerator-delete.md)