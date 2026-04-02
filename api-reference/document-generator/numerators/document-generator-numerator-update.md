# Update the documentgenerator.numerator.update

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.numerator.update` updates the numerator by its identifier.

{% note warning "" %}

You can only update a numerator that was created through the REST method `documentgenerator.numerator.add`.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the numerator.

You can obtain the identifier after [creating a numerator](./document-generator-numerator-add.md) or by using the [method to get the list of numerators](./document-generator-numerator-list.md) ||
|| **fields***
[`object`](../../data-types.md) | Set of fields to update [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | New name of the numerator ||
|| **template**
[`string`](../../data-types.md) | New number template with the placeholder `{NUMBER}`.

`{NUMBER}` should be specified in curly braces; when generating a document, the placeholder is replaced with the next counter value according to the `settings`.

Template examples:
- `{NUMBER}` → `1000`
- `INV-{NUMBER}` → `INV-1000`
- `DG/{NUMBER}/2026` → `DG/1000/2026` ||
|| **settings**
[`object`](../../data-types.md) | Updated generator settings [(detailed description)](#fields-settings) ||
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
[`integer`](../../data-types.md) | Initial counter value. Default is `1` ||
|| **step**
[`integer`](../../data-types.md) | Counter increment step. Default is `1` ||
|| **length**
[`integer`](../../data-types.md) | Minimum length of the number. Default is `0` ||
|| **padString**
[`string`](../../data-types.md) | Padding character on the left when `length > 0`. Default is `0` ||
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

{% include [Note on examples](../../../_includes/examples.md) %}

Example of updating a numerator:
- identifier — `55`
- new name — `REST Invoice Numerator Updated`
- new template — `INV-UPD-{NUMBER}`
- starting value — `2000`
- step — `10`
- minimum length of the number — `8`
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
      "id": 55,
      "fields": {
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
            "isDirectNumeration": 0
          }
        }
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.numerator.update
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "id": 55,
      "fields": {
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
            "isDirectNumeration": 0
          }
        }
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.numerator.update
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.numerator.update',
          {
              id: 55,
              fields: {
                  name: 'REST Invoice Numerator Updated',
                  template: 'INV-UPD-{NUMBER}',
                  settings: {
                      Bitrix_Main_Numerator_Generator_SequentNumberGenerator: {
                          start: 2000,
                          step: 10,
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
          'documentgenerator.numerator.update',
          [
              'id' => 55,
              'fields' => [
                  'name' => 'REST Invoice Numerator Updated',
                  'template' => 'INV-UPD-{NUMBER}',
                  'settings' => [
                      'Bitrix_Main_Numerator_Generator_SequentNumberGenerator' => [
                          'start' => 2000,
                          'step' => 10,
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
      'documentgenerator.numerator.update',
      {
          id: 55,
          fields: {
              name: 'REST Invoice Numerator Updated',
              template: 'INV-UPD-{NUMBER}',
              settings: {
                  Bitrix_Main_Numerator_Generator_SequentNumberGenerator: {
                      start: 2000,
                      step: 10,
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
      'documentgenerator.numerator.update',
      [
          'id' => 55,
          'fields' => [
              'name' => 'REST Invoice Numerator Updated',
              'template' => 'INV-UPD-{NUMBER}',
              'settings' => [
                  'Bitrix_Main_Numerator_Generator_SequentNumberGenerator' => [
                      'start' => 2000,
                      'step' => 10,
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
    },
    "time": {
        "start": 1774362882,
        "finish": 1774362882.472678,
        "duration": 0.47267794609069824,
        "processing": 0,
        "date_start": "2026-03-24T17:34:42+01:00",
        "date_finish": "2026-03-24T17:34:42+01:00",
        "operating_reset_at": 1774363482,
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
|| **id**
[`string`](../../data-types.md) | Identifier of the numerator ||
|| **name**
[`string`](../../data-types.md) | Name of the numerator ||
|| **template**
[`string`](../../data-types.md) | Number template ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the numerator. Can be `null` ||
|| **settings**
[`object`](../../data-types.md) | Settings of the numerator generators [(detailed description)](#result-settings) ||
|#

#### Object settings {#result-settings}

#|
|| **Name**
`type` | **Description** ||
|| **Bitrix_Main_Numerator_Generator_SequentNumberGenerator**
[`object`](../../data-types.md) | Settings for sequential numbering [(detailed description)](#result-settings-sequent) ||
|#

#### Object Bitrix_Main_Numerator_Generator_SequentNumberGenerator {#result-settings-sequent}

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
    "error": "DOCGEN_ACCESS_ERROR",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\Main\Numerator\Numerator constructor must be public | Required parameter `id` not provided ||
|| `400` | `100` | Could not construct parameter {numerator} | Non-existent or incorrect numerator identifier provided ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient rights to modify document generator templates ||
|| `400` | `DOCGEN_ACCESS_ERROR` | Access denied | Cannot modify a numerator that was not created through REST or a numerator of a different type ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-numerator-add.md)
- [{#T}](./document-generator-numerator-get.md)
- [{#T}](./document-generator-numerator-list.md)
- [{#T}](./document-generator-numerator-delete.md)