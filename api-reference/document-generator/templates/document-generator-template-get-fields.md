# Get the List of Fields for documentgenerator.template.getfields

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.template.getfields` returns the detail form of the template fields.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the template.

You can obtain the template identifier after [creating a template](./document-generator-template-add.md) or by using the [method to get the list of templates](./document-generator-template-list.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.template.getfields
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.template.getfields
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.template.getfields',
          {
              id: 57
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
          'documentgenerator.template.getfields',
          [
              'id' => 57,
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
      'documentgenerator.template.getfields',
      {
          id: 57
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
      'documentgenerator.template.getfields',
      [
          'id' => 57,
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
        "templateFields": {
            "DocumentNumber": {
                "title": "Number",
                "value": "9",
                "required": "Y",
                "group": [
                    "Document"
                ],
                "chain": "this.DOCUMENT.DOCUMENT_NUMBER",
                "default": "9"
            },
            "CurrentDate": {
                "value": "",
                "default": ""
            },
            "ClientName": {
                "value": "",
                "default": ""
            },
            "ClientPhone": {
                "value": "",
                "default": ""
            },
            "Total": {
                "value": "",
                "default": ""
            },
            "Comment": {
                "value": "",
                "default": ""
            },
            "UserName": {
                "value": "",
                "default": ""
            },
            "DocumentTitle": {
                "title": "Document Title",
                "value": "SUPPLY_CONTRACT_NEW Template 1773843147554 9",
                "group": [
                    "Document"
                ],
                "chain": [
                    {},
                    "getTitle"
                ],
                "required": "Y",
                "default": "SUPPLY_CONTRACT_NEW Template 1773843147554 9"
            }
        }
    },
    "time": {
        "start": 1774332782,
        "finish": 1774332783.055467,
        "duration": 1.055466890335083,
        "processing": 1,
        "date_start": "2026-03-24T09:13:02+01:00",
        "date_finish": "2026-03-24T09:13:03+01:00",
        "operating_reset_at": 1774333382,
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
|| **templateFields**
[`object`](../../data-types.md) | Object of template fields. Key — template field, value — field description [(detailed description)](#templatefield).

The set of fields in the description depends on the field type ||
|#

#### Object templateField {#templatefield}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | Field name, may be absent ||
|| **value**
[`any`](../../data-types.md) | Current value of the field ||
|| **default**
[`any`](../../data-types.md) | Default value of the field ||
|| **required**
[`char`](../../data-types.md) | Indicator of mandatory status.
-  `Y` — mandatory
-  `N` — optional ||
|| **type**
[`string`](../../data-types.md) | Field type, for example `IMAGE` ||
|| **group**
[`array`](../../data-types.md) | Groups to which the field belongs ||
|| **chain**
[`string`](../../data-types.md) \| [`array`](../../data-types.md) | Path of the field in the data provider ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Cannot get fields from deleted template"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\DocumentGenerator\Template constructor must be public | Required parameter `id` not provided ||
|| `400` | `0` | Template not found | Template with the specified `id` not found ||
|| `400` | `0` | Cannot get fields from deleted template | Cannot retrieve fields from a deleted template ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to work with templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-template-add.md)
- [{#T}](./document-generator-template-update.md)
- [{#T}](./document-generator-template-get.md)
- [{#T}](./document-generator-template-list.md)
- [{#T}](./document-generator-template-delete.md)