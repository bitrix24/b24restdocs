# Upload Template documentgenerator.template.add

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.template.add` adds a new document template.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Set of template fields [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Template name ||
|| **fileId***
[`integer`](../../data-types.md) | Identifier of the previously uploaded template file from Drive.

You can obtain the file identifier in two ways.

Use one of the file upload methods:
  - [disk.storage.uploadfile](../../disk/storage/disk-storage-upload-file.md)
  - [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:
  - [disk.storage.getchildren](../../disk/storage/disk-storage-get-children.md)
  - [disk.folder.getchildren](../../disk/folder/disk-folder-get-children.md)

||
|| **file***
[`file`](../../data-types.md#file) | Template file, if it needs to be uploaded. Used instead of the `fileId` parameter.

The file can be passed in three ways:
- as a base64 string
- as an array `["file_name.docx","base64_content"]`
- as a file `multipart/form-data`

{% note tip "Typical Use-Cases and Scenarios" %}

- [How to Upload Files](../../files/how-to-upload-files.md)

{% endnote %}

||
|| **numeratorId***
[`integer`](../../data-types.md) | Identifier of the numerator.

You can obtain the identifier after [creating a numerator](../numerators/document-generator-numerator-add.md) or by using the [method to get the list of numerators](../numerators/document-generator-numerator-list.md)
||
|| **region***
[`string`](../../data-types.md) | Template region, for example, `de` ||
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

If not provided, the current user is automatically added ||
|| **active**
[`char`](../../data-types.md) | Template activity. Possible values:
- `Y` — active
- `N` — inactive

Default is `Y` ||
|| **withStamps**
[`char`](../../data-types.md) | Include stamps and signatures. Possible values:
- `Y` — yes
- `N` — no

Default is `N` ||
|| **sort**
[`integer`](../../data-types.md) | Sort index ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"SUPPLY_CONTRACT Template","fileId":5641,"numeratorId":1,"region":"de","code":"REST_TEMPLATE","users":["UA"],"active":"Y","withStamps":"N","sort":500}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.template.add
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"SUPPLY_CONTRACT Template","fileId":5641,"numeratorId":1,"region":"de","code":"REST_TEMPLATE","users":["UA"],"active":"Y","withStamps":"N","sort":500},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.template.add
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.template.add',
          {
              fields: {
                  name: 'SUPPLY_CONTRACT Template',
                  fileId: 5641,
                  numeratorId: 1,
                  region: 'de',
                  code: 'REST_TEMPLATE',
                  users: ['UA'],
                  active: 'Y',
                  withStamps: 'N',
                  sort: 500
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
          'documentgenerator.template.add',
          [
              'fields' => [
                  'name' => 'SUPPLY_CONTRACT Template',
                  'fileId' => 5641,
                  'numeratorId' => 1,
                  'region' => 'de',
                  'code' => 'REST_TEMPLATE',
                  'users' => ['UA'],
                  'active' => 'Y',
                  'withStamps' => 'N',
                  'sort' => 500,
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
      'documentgenerator.template.add',
      {
          fields: {
              name: 'SUPPLY_CONTRACT Template',
              fileId: 5641,
              numeratorId: 1,
              region: 'de',
              code: 'REST_TEMPLATE',
              users: ['UA'],
              active: 'Y',
              withStamps: 'N',
              sort: 500
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
      'documentgenerator.template.add',
      [
          'fields' => [
              'name' => 'SUPPLY_CONTRACT Template',
              'fileId' => 5641,
              'numeratorId' => 1,
              'region' => 'de',
              'code' => 'REST_TEMPLATE',
              'users' => ['UA'],
              'active' => 'Y',
              'withStamps' => 'N',
              'sort' => 500,
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
            "name": "SUPPLY_CONTRACT Template",
            "region": "de",
            "code": "REST_TEMPLATE",
            "download": "https://mysite.com/bitrix/services/main/ajax.php?action=documentgenerator.api.template.download&SITE_ID=s1&id=57&ts=0",
            "active": "Y",
            "moduleId": "rest",
            "numeratorId": "1",
            "withStamps": "N",
            "providers": {
                "bitrix\\documentgenerator\\dataprovider\\rest": "bitrix\\documentgenerator\\dataprovider\\rest"
            },
            "users": {
                "UA": "UA"
            },
            "isDeleted": "N",
            "sort": "500",
            "createTime": "2026-03-23T16:51:25+01:00",
            "updateTime": "2026-03-23T16:51:25+01:00",
            "downloadMachine": "https://mysite.com/rest/documentgenerator.api.template.download.json?auth=6d53c1690000071b00000844000001f7f0f1075612a492ef6fe0b4127e521b543e4376&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS50ZW1wbGF0ZS5kb3dubG9hZCZTSVRFX0lEPXMxJmlkPTU3JnRzPTAmXz1IeG56TmtsQklGNjRsQlBkNkNiekNwd1U2bnduQk40Mw%3D%3D%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS50ZW1wbGF0ZS5kb3dubG9hZCZkb2N1bWVudGdlbmVyYXRvcnxZV04wYVc5dVBXUnZZM1Z0Wlc1MFoyVnVaWEpoZEc5eUxtRndhUzUwWlcxd2JHRjBaUzVrYjNkdWJHOWhaQ1pUU1ZSRlgwbEVQWE14Sm1sa1BUVTNKblJ6UFRBbVh6MUllRzU2VG10c1FrbEdOalJzUWxCa05rTmlla053ZDFVMmJuZHVRazQwTXc9PXw2ZDUzYzE2OTAwMDAwNzFiMDAwMDA4NDQwMDAwMDFmN2YwZjEwNzU2MTJhNDkyZWY2ZmUwYjQxMjdlNTIxYjU0M2U0Mzc2Ig%3D%3D.vtTu%2B9Ac%2BT5VgeyDm1jiVBGsmCQagvrqjACd%2BNgDigA%3D"
        }
    },
    "time": {
        "start": 1774273885,
        "finish": 1774273885.843453,
        "duration": 0.8434529304504395,
        "processing": 0,
        "date_start": "2026-03-23T16:51:25+01:00",
        "date_finish": "2026-03-23T16:51:25+01:00",
        "operating_reset_at": 1774274485,
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
[`char`](../../data-types.md) | Template activity. Possible values:
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
[`object`](../../data-types.md) | Template data providers. 

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
[`datetime`](../../data-types.md) | Creation date and time ||
|| **updateTime**
[`datetime`](../../data-types.md) | Update date and time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Empty required fields: numeratorId"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `400` | `100` | Invalid value to match with parameter {fields}. Should be value of type array | Parameter `fields` not passed as an object ||
|| `400` | `0` | Empty required fields: name, numeratorId, region | Required fields for creating a template not provided: `name`, `numeratorId`, `region`. The error lists the fields that are missing in the request ||
|| `400` | `0` | Missing file content | Template file not provided ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to modify templates ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-template-update.md)
- [{#T}](./document-generator-template-get.md)
- [{#T}](./document-generator-template-list.md)
- [{#T}](./document-generator-template-delete.md)
- [{#T}](./document-generator-template-get-fields.md)