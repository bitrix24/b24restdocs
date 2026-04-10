# Update Template documentgenerator.template.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.template.update` updates an existing template.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Template identifier.

You can obtain the template identifier after [creating a template](./document-generator-template-add.md) or by using the [method to retrieve the list of templates](./document-generator-template-list.md) ||
|| **fields***
[`object`](../../data-types.md) | Template fields to update [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Template name ||
|| **file**
[`file`](../../data-types.md#file) | New template file. It can be passed in three ways:
- as a base64 string
- as an array `["file_name.docx","base64_content"]`
- as a file `multipart/form-data`

{% note tip "Typical use-cases and scenarios" %}

- [How to upload files](../../files/how-to-upload-files.md)

{% endnote %}
||
|| **numeratorId**
[`integer`](../../data-types.md) | Identifier of the numerator ||
|| **region**
[`string`](../../data-types.md) | Template region, for example `de` ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the template ||
|| **users**
[`array`](../../data-types.md) | Users who will have access to the template.

Possible values:

- `U{id}` — user
- `G{id}` — user group
- `AU` — all authorized users
- `UA` — all users
- `D{id}` — department
- `DR{id}` — department with sub-departments
- `SG{id}` — working group or project
- `SG{id}_A` — owner of the working group or project
- `SG{id}_E` — moderators of the working group or project
- `SG{id}_K` — all members of the working group or project 
||
|| **providers**
[`array`](../../data-types.md) | List of data providers ||
|| **active**
[`char`](../../data-types.md) | Activity status. Possible values:
- `Y` — active
- `N` — inactive
||
|| **withStamps**
[`char`](../../data-types.md) | Include stamps and signatures. Possible values:
- `Y` — yes
- `N` — no
||
|| **sort**
[`integer`](../../data-types.md) | Sort index ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57,"fields":{"name":"SUPPLY_CONTRACT_NEW Template","numeratorId":3,"code":"REST_TEMPLATE","users":["U503"],"sort":700}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.template.update
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57,"fields":{"name":"SUPPLY_CONTRACT_NEW Template","numeratorId":3,"code":"REST_TEMPLATE","users":["U503"],"sort":700},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.template.update
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.template.update',
          {
              id: 57,
              fields: {
                  name: 'SUPPLY_CONTRACT_NEW Template',
                  numeratorId: 3,
                  code: 'REST_TEMPLATE',
                  users: ['U503'],
                  sort: 700
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
          'documentgenerator.template.update',
          [
              'id' => 57,
              'fields' => [
                  'name' => 'SUPPLY_CONTRACT_NEW Template',
                  'numeratorId' => 3,
                  'code' => 'REST_TEMPLATE',
                  'users' => ['U503'],
                  'sort' => 700,
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
      'documentgenerator.template.update',
      {
          id: 57,
          fields: {
              name: 'SUPPLY_CONTRACT_NEW Template',
              numeratorId: 3,
              code: 'REST_TEMPLATE',
              users: ['U503'],
              sort: 700
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
      'documentgenerator.template.update',
      [
          'id' => 57,
          'fields' => [
              'name' => 'SUPPLY_CONTRACT_NEW Template',
              'numeratorId' => 3,
              'code' => 'REST_TEMPLATE',
              'users' => ['U503'],
              'sort' => 700,
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
        "template": {
            "id": "57",
            "name": "SUPPLY_CONTRACT_NEW Template",
            "region": "de",
            "code": "REST_TEMPLATE",
            "download": "https://mysite.com/bitrix/services/main/ajax.php?action=documentgenerator.api.template.download&SITE_ID=s1&id=57&ts=0",
            "active": "Y",
            "moduleId": "rest",
            "numeratorId": "3",
            "withStamps": "N",
            "providers": {
                "bitrix\\documentgenerator\\dataprovider\\rest": "bitrix\\documentgenerator\\dataprovider\\rest"
            },
            "users": {
                "U503": "U503"
            },
            "isDeleted": "N",
            "sort": "700",
            "createTime": "2026-03-23T16:51:25+02:00",
            "updateTime": "2026-03-23T17:26:39+02:00",
            "downloadMachine": "https://mysite.com/rest/documentgenerator.api.template.download.json?auth=6d53c1690000071b00000844000001f7f0f1075612a492ef6fe0b4127e521b543e4376&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS50ZW1wbGF0ZS5kb3dubG9hZCZTSVRFX0lEPXMxJmlkPTU3JnRzPTAmXz1UdDBpNDBOU2hvQkxtN1Y5VmhEZm1MbTU2ZmQ0d3puNA%3D%3D%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS50ZW1wbGF0ZS5kb3dubG9hZHxkb2N1bWVudGdlbmVyYXRvcnxZV04wYVc5dVBXUnZZM1Z0Wlc1MFoyVnVaWEpoZEc5eUxtRndhUzUwWlcxd2JHRjBaUzVrYjNkdWJHOWhaQ1pUU1ZSRlgwbEVQWE14Sm1sa1BUVTNKblJ6UFRBbVh6MVVkREJwTkRCT1UyaHZRa3h0TjFZNVZtaEVabTFNYlRVMlptUTBkM3B1TkE9PXw2ZDUzYzE2OTAwMDAwNzFiMDAwMDA4NDQwMDAwMDFmN2YwZjEwNzU2MTJhNDkyZWY2ZmUwYjQxMjdlNTIxYjU0M2U0Mzc2Ig%3D%3D.6kAmDGdiQCSZI%2Bcm%2Fg86RUqVH5dMvSDxHbx20yjOYK4%3D"
        }
    },
    "time": {
        "start": 1774275999,
        "finish": 1774275999.631086,
        "duration": 0.6310861110687256,
        "processing": 0,
        "date_start": "2026-03-23T17:26:39+02:00",
        "date_finish": "2026-03-23T17:26:39+02:00",
        "operating_reset_at": 1774276599,
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
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **template**
[`object`](../../data-types.md) | Template data [(detailed description)](#template) ||
|#

#### Object template {#template}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Template identifier ||
|| **name**
[`string`](../../data-types.md) | Template name ||
|| **region**
[`string`](../../data-types.md) | Template region ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the template ||
|| **download**
[`string`](../../data-types.md) | Link to download the template ||
|| **downloadMachine**
[`string`](../../data-types.md) | Link to download the template for machine processing ||
|| **active**
[`char`](../../data-types.md) | Template activity status. Possible values:
- `Y` — active
- `N` — inactive
||
|| **moduleId**
[`string`](../../data-types.md) | Module identifier ||
|| **numeratorId**
[`string`](../../data-types.md) | Identifier of the numerator ||
|| **withStamps**
[`char`](../../data-types.md) | Use of stamps and signatures. Possible values:
- `Y` — yes
- `N` — no
||
|| **providers**
[`object`](../../data-types.md) | Data providers of the template. 

Key and value are equal to the name of the data provider class ||
|| **users**
[`object`](../../data-types.md) | Access codes to the template. 

Key and value are equal to the access code ||
|| **isDeleted**
[`char`](../../data-types.md) | Deletion status of the template. Possible values:
- `Y` — deleted
- `N` — not deleted
||
|| **sort**
[`string`](../../data-types.md) | Sort index ||
|| **createTime**
[`datetime`](../../data-types.md) | Date and time of creation ||
|| **updateTime**
[`datetime`](../../data-types.md) | Date and time of update ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Template not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\DocumentGenerator\Template constructor must be public | Required parameter `id` not provided ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `400` | `0` | Template not found | Template with the specified `id` not found ||
|| `400` | `0` | You do not have permissions to modify this template | Insufficient rights to modify the specified template ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient rights to modify templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-template-add.md)
- [{#T}](./document-generator-template-get.md)
- [{#T}](./document-generator-template-list.md)
- [{#T}](./document-generator-template-delete.md)
- [{#T}](./document-generator-template-get-fields.md)