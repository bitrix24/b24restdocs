# Upload Template documentgenerator.template.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.template.add` adds a document template from a `.docx` file.

{% note info "" %}

The method creates templates only for the `rest` module: `moduleId` in the response is always `rest`, and the data provider is `Bitrix\DocumentGenerator\DataProvider\Rest`.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

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
[`string`](../../data-types.md) | Template name, 1 to 100 characters ||
|| **file***
[`file`](../../data-types.md#standart-types) | Template file in `.docx` format. Required if `fileId` is not passed.

The file can be passed in three ways:
- as a [Base64 String](../../files/how-to-upload-files.md#string) — the file name is not passed in this format
- as an [Array](../../files/how-to-upload-files.md#array) `["file_name.docx","base64_content"]`
- as a `multipart/form-data` file with the field name `file`

If `fileId` is passed, the `file` parameter is not used ||
|| **fileId**
[`integer`](../../data-types.md) | Identifier of the template file in the document generator. Used instead of `file` to create a template based on the file of an existing template.

You can retrieve the identifier from the `fileId` field using the [documentgenerator.template.list](./document-generator-template-list.md) method. A Drive file identifier does not work here: it is a different identifier ||
|| **numeratorId***
[`integer`](../../data-types.md) | Identifier of the numerator.

You can obtain the identifier after [creating a numerator](../numerators/document-generator-numerator-add.md) or by using the [method to get the list of numerators](../numerators/document-generator-numerator-list.md) ||
|| **region***
[`string`](../../data-types.md) | Template region code, for example `de`.

You can retrieve the list of regions using the [documentgenerator.region.list](../region/document-generator-region-list.md) method ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the template ||
|| **users**
[`array`](../../data-types.md) | Access codes of the users who have access to the template.

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

If the parameter is not passed or an empty array is passed, the current user gets access in the `U{id}` format ||
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
[`integer`](../../data-types.md) | Sort index. Default is `500` ||
|| **timezone**
[`string`](../../data-types.md) | Time zone used to display dates in documents generated from the template. Pass an IANA time zone identifier, for example `Europe/Berlin`.

If the field is not passed or is an empty string, dates are displayed in the shared time zone set in the document template settings, or in the server time zone if none is selected ||
|#

{% note tip "" %}

To learn how to prepare a file and choose a transfer format, see [How to Upload Files](../../files/how-to-upload-files.md).

{% endnote %}

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"SUPPLY_CONTRACT Template","file":["supply_contract.docx","base64_encoded_content_here"],"numeratorId":1,"region":"de","code":"REST_TEMPLATE","users":["UA"],"active":"Y","withStamps":"N","sort":500,"timezone":"Europe/Berlin"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.template.add
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"SUPPLY_CONTRACT Template","file":["supply_contract.docx","base64_encoded_content_here"],"numeratorId":1,"region":"de","code":"REST_TEMPLATE","users":["UA"],"active":"Y","withStamps":"N","sort":500,"timezone":"Europe/Berlin"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.template.add
  ```

- JS (TS)

  ```ts
  // This snippet is an ES module: top-level await requires type="module" or a bundler.
  // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
  import { Text } from '@bitrix24/b24jssdk'
  import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

  declare const $b24: B24Frame

  // Shape of the payload returned in result (match the "response handling" section of the page)
  type TemplateAddResult = {
    template: {
      id: string
      name: string
      region: string
      code: string
      download: string
      downloadMachine: string
      active: 'Y' | 'N'
      moduleId: string
      numeratorId: string
      withStamps: 'Y' | 'N'
      providers: Record<string, string>
      users: Record<string, string>
      isDeleted: 'Y' | 'N'
      sort: string
      createTime: ISODate
      updateTime: ISODate
      timezone: string | null
    }
  }

  try {
    const response = await $b24.actions.v2.call.make<TemplateAddResult>({
      method: 'documentgenerator.template.add',
      params: {
        fields: {
          name: 'SUPPLY_CONTRACT Template',
          file: ['supply_contract.docx', 'base64_encoded_content_here'],
          numeratorId: 1,
          region: 'de',
          code: 'REST_TEMPLATE',
          users: ['UA'],
          active: 'Y',
          withStamps: 'N',
          sort: 500,
          timezone: 'Europe/Berlin',
        },
      },
      requestId: Text.getUuidRfc4122()
    })

    // The payload is available only on a successful response
    if (!response.isSuccess) {
      console.error(response.getErrorMessages().join('; '))
    } else {
      const result = response.getData()!.result
      console.info('Added template:', result.template.id, result.template.name)
    }
  } catch (error) {
    // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
    console.error(error)
  }
  ```

- JS (UMD)

  ```html
  <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
  <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
  <script>
    async function addTemplate() {
      try {
        // Initialize the SDK inside a Bitrix24 frame
        const $b24 = await B24Js.initializeB24Frame()

        const response = await $b24.actions.v2.call.make({
          method: 'documentgenerator.template.add',
          params: {
            fields: {
              name: 'SUPPLY_CONTRACT Template',
              file: ['supply_contract.docx', 'base64_encoded_content_here'],
              numeratorId: 1,
              region: 'de',
              code: 'REST_TEMPLATE',
              users: ['UA'],
              active: 'Y',
              withStamps: 'N',
              sort: 500,
              timezone: 'Europe/Berlin',
            },
          },
          requestId: B24Js.Text.getUuidRfc4122()
        })

        // The payload is available only on a successful response
        if (!response.isSuccess) {
          console.error(response.getErrorMessages().join('; '))
          return
        }

        const result = response.getData().result
        console.info('Added template:', result.template.id, result.template.name)
      } catch (error) {
        // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
        console.error(error)
      }
    }

    document.addEventListener('DOMContentLoaded', addTemplate)
  </script>
  ```

- Python

  ```python
  from b24pysdk.errors import BitrixAPIError, BitrixSDKException

  template_fields = {
      "name": "SUPPLY_CONTRACT Template",
      "file": [
          "supply_contract.docx",
          "base64_encoded_content_here",
      ],
      "numeratorId": 1,
      "region": "de",
      "code": "REST_TEMPLATE",
      "users": [
          "UA",
      ],
      "active": "Y",
      "withStamps": "N",
      "sort": 500,
      "timezone": "Europe/Berlin",
  }

  try:
      bitrix_response = client.documentgenerator.template.add(
          fields=template_fields,
      ).response
      result = bitrix_response.result
      print(result)
  except BitrixAPIError as error:
      print(
          "Bitrix API error",
          f"error: {error.error}",
          f"error_description: {error.error_description}",
          sep="\n",
      )
  except BitrixSDKException as error:
      print(f"Bitrix SDK error: {error.message}")
  except Exception as error:
      print(f"Unexpected error: {error}")
  ```

- PHP

  ```php
  try {
      $response = $b24Service->core->call(
          'documentgenerator.template.add',
          [
              'fields' => [
                  'name' => 'SUPPLY_CONTRACT Template',
                  'file' => ['supply_contract.docx', 'base64_encoded_content_here'],
                  'numeratorId' => 1,
                  'region' => 'de',
                  'code' => 'REST_TEMPLATE',
                  'users' => ['UA'],
                  'active' => 'Y',
                  'withStamps' => 'N',
                  'sort' => 500,
                  'timezone' => 'Europe/Berlin',
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
              file: ['supply_contract.docx', 'base64_encoded_content_here'],
              numeratorId: 1,
              region: 'de',
              code: 'REST_TEMPLATE',
              users: ['UA'],
              active: 'Y',
              withStamps: 'N',
              sort: 500,
              timezone: 'Europe/Berlin'
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
              'file' => ['supply_contract.docx', 'base64_encoded_content_here'],
              'numeratorId' => 1,
              'region' => 'de',
              'code' => 'REST_TEMPLATE',
              'users' => ['UA'],
              'active' => 'Y',
              'withStamps' => 'N',
              'sort' => 500,
              'timezone' => 'Europe/Berlin',
          ],
      ]
  );

  print_r($result);
  ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "documentgenerator.template.add", b24.Params{
    	"fields": b24.Params{
    		"name":        "SUPPLY_CONTRACT Template",
    		"file":        []string{"supply_contract.docx", "base64_encoded_content_here"},
    		"numeratorId": 1,
    		"region":      "de",
    		"code":        "REST_TEMPLATE",
    		"users":       []string{"UA"},
    		"active":      "Y",
    		"withStamps":  "N",
    		"sort":        500,
    		"timezone":    "Europe/Berlin",
    	},
    })
    if err != nil {
    	return fmt.Errorf("documentgenerator.template.add: %w", err)
    }

    // The method wraps the response in an object with the "template" key.
    raw, ok := b24.Unwrap(res.Result, "template")
    if !ok {
    	return fmt.Errorf("no template key in the response")
    }

    var item struct {
    	ID       b24.ID `json:"id"`
    	Name     string `json:"name"`
    	Region   string `json:"region"`
    	Code     string `json:"code"`
    	Download string `json:"download"`
    	Active   string `json:"active"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "template": {
            "id": "385",
            "name": "SUPPLY_CONTRACT Template",
            "region": "de",
            "code": "REST_TEMPLATE",
            "download": "https://mysite.com/bitrix/services/main/ajax.php?action=documentgenerator.api.template.download&SITE_ID=s1&id=385&ts=1794393725",
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
            "createTime": "2026-11-11T11:42:03+01:00",
            "updateTime": "2026-11-11T11:42:03+01:00",
            "timezone": "Europe/Berlin",
            "downloadMachine": "https://mysite.com/rest/1/xxxxxxxxxxxxxxxx/documentgenerator.api.template.download/?token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS50ZW1wbGF0ZS5kb3dubG9hZCZTSVRFX0lEPXMxJmlkPTM4NSZ0cz0xNzk0MzkzNzI1Jl89ZjI1VkdnOFlydkdPQUZVRnFpd05NRDBnN3JTTDIySjI%3D%7C...%3D"
        }
    },
    "time": {
        "start": 1794393723,
        "finish": 1794393725.630273,
        "duration": 2.6302731037139893,
        "processing": 2,
        "date_start": "2026-11-11T11:42:03+01:00",
        "date_finish": "2026-11-11T11:42:05+01:00",
        "operating_reset_at": 1794393960,
        "operating": 60.87245774269104
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
[`string`](../../data-types.md) | Link to download the template file in the browser of a user authorized in Bitrix24 ||
|| **downloadMachine**
[`string`](../../data-types.md) | Link to download the template file with an access token. Suitable for downloading from an app or script without a browser session ||
|| **active**
[`char`](../../data-types.md) | Template activity. Possible values:
- `Y` — active
- `N` — inactive
||
|| **moduleId**
[`string`](../../data-types.md) | Identifier of the template module. For templates created by this method, it is always `rest` ||
|| **numeratorId**
[`string`](../../data-types.md) | Identifier of the numerator ||
|| **withStamps**
[`char`](../../data-types.md) | Use of stamps and signatures. Possible values:
- `Y` — yes
- `N` — no
||
|| **providers**
[`object`](../../data-types.md) | Template data providers.

Key and value are equal to the name of the data provider class in lowercase, for example `bitrix\documentgenerator\dataprovider\rest` ||
|| **users**
[`object`](../../data-types.md) | Access codes to the template.

Key and value are equal to the access code, for example `UA` or `U1` ||
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
|| **timezone**
[`string`](../../data-types.md) | Template time zone in IANA format, for example `Europe/Berlin`.

`null` means the template has no time zone of its own: dates are displayed in the shared time zone set in the document template settings, or in the server time zone if none is selected ||
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
|| `400` | `100` | Invalid value {abc} to match with parameter {fields}. Should be value of type array. | Parameter `fields` not passed as an object. The passed value is shown in curly braces ||
|| `400` | `0` | Missing file content | Neither `file` nor `fileId` provided ||
|| `400` | `0` | Could not save file | Parameter `file` passed in an invalid format, for example an array without the file content ||
|| `400` | `0` | Empty required fields: name, numeratorId, region | Required fields not provided. The error text lists only the missing fields out of `name`, `numeratorId`, `region` ||
|| `400` | `TIMEZONE_INVALID` | Timezone must be a canonical IANA identifier. | The `timezone` field contains a value that is not an IANA time zone identifier, for example the offset `+01:00` ||
|| `400` | `BX_INVALID_VALUE` | The length of "NAME" should not exceed 100 characters. | The `name` value is longer than 100 characters ||
|| `400` | `BX_INVALID_VALUE` | The value of "ACTIVE" is invalid. | The `active` field contains a value other than `Y` and `N` ||
|| `400` | `BX_INVALID_VALUE` | The value of "WITH_STAMPS" is invalid. | The `withStamps` field contains a value other than `Y` and `N` ||
|| `400` | `0` | You do not have permissions to modify templates | The user does not have permission to modify document generator templates ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-template-update.md)
- [{#T}](./document-generator-template-get.md)
- [{#T}](./document-generator-template-list.md)
- [{#T}](./document-generator-template-delete.md)
- [{#T}](./document-generator-template-get-fields.md)