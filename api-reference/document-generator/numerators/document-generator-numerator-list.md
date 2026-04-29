# Get the List of Numerators documentgenerator.numerator.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.numerator.list` returns a list of numerators for the document generator. It retrieves all numerators available in the current Bitrix24, including [CRM numerators](../../crm/document-generator/numerator/index.md).

## Method Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static: 50 items.

To select the second page of results, you must pass the value `50`. To select the third page of results, you must pass `100`, and so on.

The formula for calculating the `start` value:

`start = (N - 1) * 50`, where `N` is the desired page number ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.numerator.list
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.numerator.list
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.numerator.list',
          {
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
          'documentgenerator.numerator.list',
          [
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
      'documentgenerator.numerator.list',
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

              if (result.more())
              {
                  result.next();
              }
          }
      }
  );
  ```

- PHP CRest

  ```php
  require_once('crest.php');

  $result = CRest::call(
      'documentgenerator.numerator.list',
      [
          'start' => 0,
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
        "numerators": [
            ..., // description of other numerators, including CRM
            {
                "id": "53",
                "name": "General Number",
                "template": "{NUMBER}",
                "settings": {
                    "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
                        "start": 1,
                        "step": 1,
                        "length": 0,
                        "padString": "0",
                        "periodicBy": null,
                        "timezone": null,
                        "isDirectNumeration": false
                    }
                }
            },
            {
                "id": "55",
                "name": "REST Invoice Numerator Updated",
                "template": "INV-UPD-{NUMBER}",
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
        ]
    },
    "total": 20,
    "time": {
        "start": 1774363478,
        "finish": 1774363478.809978,
        "duration": 0.8099780082702637,
        "processing": 0,
        "date_start": "2026-03-24T17:44:38+01:00",
        "date_finish": "2026-03-24T17:44:38+01:00",
        "operating_reset_at": 1774364078,
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
|| **total**
[`integer`](../../data-types.md) | The number of numerators in the current selection ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#| 
|| **Name**
`type` | **Description** ||
|| **numerators**
[`array`](../../data-types.md) | List of numerators [(detailed description)](#numerators) ||
|#

#### Numerators Array Element {#numerators}

#| 
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the numerator ||
|| **name**
[`string`](../../data-types.md) | Name of the numerator ||
|| **template**
[`string`](../../data-types.md) | Number template ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the numerator. Can be `null` ||
|| **settings**
[`object`](../../data-types.md) | Settings of the numerator generators [(detailed description)](#numerators-settings) ||
|#

#### Settings Object {#numerators-settings}

#| 
|| **Name**
`type` | **Description** ||
|| **Bitrix_Main_Numerator_Generator_SequentNumberGenerator**
[`object`](../../data-types.md) | Settings for sequential numbering [(detailed description)](#numerators-settings-sequent) ||
|#

#### Bitrix_Main_Numerator_Generator_SequentNumberGenerator Object {#numerators-settings-sequent}

#| 
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | Initial value of the counter ||
|| **step**
[`integer`](../../data-types.md) | Increment step of the counter ||
|| **length**
[`integer`](../../data-types.md) | Minimum length of the number ||
|| **padString**
[`string`](../../data-types.md) | Padding character on the left when `length > 0` ||
|| **periodicBy**
[`string`](../../data-types.md) | Reset period of the counter. Can be `null` ||
|| **timezone**
[`string`](../../data-types.md) | Timezone identifier for periodic reset. Can be `null` ||
|| **isDirectNumeration**
[`boolean`](../../data-types.md) | Indicator of direct numbering ||
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
|| `400` | `0` | You do not have permissions to modify templates | Insufficient rights to modify document generator templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-numerator-add.md)
- [{#T}](./document-generator-numerator-update.md)
- [{#T}](./document-generator-numerator-get.md)
- [{#T}](./document-generator-numerator-delete.md)