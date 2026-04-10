# Add Document Generator Numerator: documentgenerator.numerator.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.numerator.add` creates a document numerator.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Set of numerator parameters [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the numerator ||
|| **template***
[`string`](../../data-types.md) | Number template with the placeholder `{NUMBER}`.

The placeholder `{NUMBER}` should be specified in curly braces. When generating a document, the placeholder is replaced with the next counter value according to the `settings`.

Template examples:
- `{NUMBER}` → `1000`
- `INV-{NUMBER}` → `INV-1000`
- `DG/{NUMBER}/2026` → `DG/1000/2026` 
||
|| **settings**
[`object`](../../data-types.md) | Settings for the numerator generators [(detailed description)](#fields-settings) ||
|#

#### Parameter settings {#fields-settings}

#|
|| **Name**
`type` | **Description** ||
|| **Bitrix_Main_Numerator_Generator_SequentNumberGenerator**
[`object`](../../data-types.md) | Settings for sequential numbering [(detailed description)](#fields-settings-sequent) ||
|#

#### Parameters Bitrix_Main_Numerator_Generator_SequentNumberGenerator {#fields-settings-sequent}

#|
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | Initial counter value.

Default is `1` ||
|| **step**
[`integer`](../../data-types.md) | Counter increment step.

Default is `1` ||
|| **length**
[`integer`](../../data-types.md) | Minimum length of the number.

Default is `0` ||
|| **padString**
[`string`](../../data-types.md) | Padding character on the left when `length > 0`.

Default is `0` ||
|| **periodicBy**
[`string`](../../data-types.md) | Counter reset period:
- `''` — no reset
- `day` — daily
- `month` — monthly
- `year` — yearly ||
|| **timezone**
[`string`](../../data-types.md) | Timezone identifier for periodic reset, e.g., `Europe/Berlin` ||
|| **isDirectNumeration**
[`boolean`](../../data-types.md) | Direct numbering flag.

Possible values:
- `0` — disabled
- `1` — enabled

Default is `0`. 

In the method response, the value is returned as `true` | `false` ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

Example of creating a numerator:
- name — `REST Invoice Numerator`
- template — `INV-{NUMBER}`
- starting value — `1000`
- step — `5`
- minimum number length — `8`
- padding character — `0`
- reset number — yearly `year`
- timezone — `Europe/Berlin`
- direct numbering — `0`

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "fields": {
        "name": "REST Invoice Numerator",
        "template": "INV-{NUMBER}",
        "settings": {
          "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
            "start": 1000,
            "step": 5,
            "length": 8,
            "padString": "0",
            "periodicBy": "year",
            "timezone": "Europe/Berlin",
            "isDirectNumeration": 0
          }
        }
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.numerator.add
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "fields": {
        "name": "REST Invoice Numerator",
        "template": "INV-{NUMBER}",
        "settings": {
          "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
            "start": 1000,
            "step": 5,
            "length": 8,
            "padString": "0",
            "periodicBy": "year",
            "timezone": "Europe/Berlin",
            "isDirectNumeration": 0
          }
        }
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.numerator.add
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.numerator.add',
          {
              fields: {
                  name: 'REST Invoice Numerator',
                  template: 'INV-{NUMBER}',
                  settings: {
                      Bitrix_Main_Numerator_Generator_SequentNumberGenerator: {
                          start: 1000,
                          step: 5,
                          length: 8,
                          padString: '0',
                          periodicBy: 'year',
                          timezone: 'Europe/Berlin',
                          isDirectNumeration: 0
                      }
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
          'documentgenerator.numerator.add',
          [
              'fields' => [
                  'name' => 'REST Invoice Numerator',
                  'template' => 'INV-{NUMBER}',
                  'settings' => [
                      'Bitrix_Main_Numerator_Generator_SequentNumberGenerator' => [
                          'start' => 1000,
                          'step' => 5,
                          'length' => 8,
                          'padString' => '0',
                          'periodicBy' => 'year',
                          'timezone' => 'Europe/Berlin',
                          'isDirectNumeration' => 0,
                      ],
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
      'documentgenerator.numerator.add',
      {
          fields: {
              name: 'REST Invoice Numerator',
              template: 'INV-{NUMBER}',
              settings: {
                  Bitrix_Main_Numerator_Generator_SequentNumberGenerator: {
                      start: 1000,
                      step: 5,
                      length: 8,
                      padString: '0',
                      periodicBy: 'year',
                      timezone: 'Europe/Berlin',
                      isDirectNumeration: 0
                  }
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
      'documentgenerator.numerator.add',
      [
          'fields' => [
              'name' => 'REST Invoice Numerator',
              'template' => 'INV-{NUMBER}',
              'settings' => [
                  'Bitrix_Main_Numerator_Generator_SequentNumberGenerator' => [
                      'start' => 1000,
                      'step' => 5,
                      'length' => 8,
                      'padString' => '0',
                      'periodicBy' => 'year',
                      'timezone' => 'Europe/Berlin',
                      'isDirectNumeration' => 0,
                  ],
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
        "numerator": {
            "name": "REST Invoice Numerator",
            "template": "INV-{NUMBER}",
            "id": 55,
            "code": null,
            "settings": {
                "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
                    "start": 1000,
                    "step": 5,
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
        "start": 1774360939,
        "finish": 1774360939.968711,
        "duration": 0.9687108993530273,
        "processing": 0,
        "date_start": "2026-03-24T17:02:19+02:00",
        "date_finish": "2026-03-24T17:02:19+02:00",
        "operating_reset_at": 1774361539,
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
|| **numerator**
[`object`](../../data-types.md) | Created numerator [(detailed description)](#result-numerator) ||
|#

#### Object numerator {#result-numerator}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the numerator ||
|| **name**
[`string`](../../data-types.md) | Name of the numerator ||
|| **template**
[`string`](../../data-types.md) | Number template ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the numerator. Can be `null` ||
|| **settings**
[`object`](../../data-types.md) | Settings for the numerator generators [(detailed description)](#result-numerator-settings) ||
|#

#### Object settings {#result-numerator-settings}

#|
|| **Name**
`type` | **Description** ||
|| **Bitrix_Main_Numerator_Generator_SequentNumberGenerator**
[`object`](../../data-types.md) | Settings for sequential numbering [(detailed description)](#result-numerator-settings-sequent) ||
|#

#### Object Bitrix_Main_Numerator_Generator_SequentNumberGenerator {#result-numerator-settings-sequent}

#|
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | Initial counter value ||
|| **step**
[`integer`](../../data-types.md) | Counter increment step ||
|| **length**
[`integer`](../../data-types.md) | Minimum length of the number ||
|| **padString**
[`string`](../../data-types.md) | Padding character on the left when `length > 0` ||
|| **periodicBy**
[`string`](../../data-types.md) | Counter reset period ||
|| **timezone**
[`string`](../../data-types.md) | Timezone identifier for periodic reset ||
|| **isDirectNumeration**
[`boolean`](../../data-types.md) | Direct numbering flag ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Empty required fields: name, template"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `400` | `0` | Empty required fields: name, template | Required fields `name` and `template` not provided within `fields` ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to modify document generator templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-numerator-update.md)
- [{#T}](./document-generator-numerator-get.md)
- [{#T}](./document-generator-numerator-list.md)
- [{#T}](./document-generator-numerator-delete.md)