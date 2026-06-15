# Get Template by ID documentgenerator.template.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.template.get` returns information about a template by its ID.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Template ID.

You can obtain the template ID after [creating a template](./document-generator-template-add.md) or by using the [get template list method](./document-generator-template-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.template.get
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.template.get
  ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TemplateGetResult = {
      template: {
        id: string
        name: string
        region: string
        code: string
        download: string
        active: string
        moduleId: string
        numeratorId: string
        withStamps: string
        providers: Record<string, string>
        users: Record<string, string>
        isDeleted: string
        sort: string
        createTime: ISODate | null
        updateTime: ISODate | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<TemplateGetResult>({
        method: 'documentgenerator.template.get',
        params: {
          id: 57,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.template.id, result.template.name, result.template.active)
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
      async function getTemplate() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'documentgenerator.template.get',
            params: {
              id: 57,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.template.id, result.template.name, result.template.active)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTemplate)
    </script>
    ```

- PHP

  ```php
  try {
      $response = $b24Service->core->call(
          'documentgenerator.template.get',
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
      'documentgenerator.template.get',
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
      'documentgenerator.template.get',
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
            "downloadMachine": "https://mysite.com/rest/documentgenerator.api.template.download.json?auth=6d53c1690000071b00000844000001f7f0f1075612a492ef6fe0b4127e521b543e4376&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS50ZW1wbGF0ZS5kb3dubG9hZCZTSVRFX0lEPXMxJmlkPTU3JnRzPTAmXz00a1NkZTRVdWtNZk1XODVrVlczS2dTc2x5c1lyMmRGag%3D%3D%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS50ZW1wbGF0ZS5kb3dubG9hZHxkb2N1bWVudGdlbmVyYXRvcnxZV04wYVc5dVBXUnZZM1Z0Wlc1MFoyVnVaWEpoZEc5eUxtRndhUzUwWlcxd2JHRjBaUzVrYjNkdWJHOWhaQ1pUU1ZSRlgwbEVQWE14Sm1sa1BUVTNKblJ6UFRBbVh6MDBhMU5rWlRSVmRXdE5aazFYT0RWclZsY3pTMmRUYzJ4NWMxbHlNbVJHYWc9PXw2ZDUzYzE2OTAwMDAwNzFiMDAwMDA4NDQwMDAwMDFmN2YwZjEwNzU2MTJhNDkyZWY2ZmUwYjQxMjdlNTIxYjU0M2U0Mzc2Ig%3D%3D.9JnHSEVzH2jcCA1zZA%2BlsYJCTGs6V3dGLNsnjfbnuNs%3D"
        }
    },
    "time": {
        "start": 1774276994,
        "finish": 1774276994.504076,
        "duration": 0.5040760040283203,
        "processing": 0,
        "date_start": "2026-03-23T17:43:14+02:00",
        "date_finish": "2026-03-23T17:43:14+02:00",
        "operating_reset_at": 1774277594,
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
|| **template**
[`object`](../../data-types.md) | Template data [(detailed description)](#template) ||
|#

#### Template Object {#template}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Template ID ||
|| **name**
[`string`](../../data-types.md) | Template name ||
|| **region**
[`string`](../../data-types.md) | Template region ||
|| **code**
[`string`](../../data-types.md) | Template symbolic code ||
|| **download**
[`string`](../../data-types.md) | Link to download the template file ||
|| **active**
[`char`](../../data-types.md) | Template activity. Possible values:
- `Y` — active
- `N` — inactive
||
|| **moduleId**
[`string`](../../data-types.md) | Module code to which the template is linked ||
|| **numeratorId**
[`string`](../../data-types.md) | Numerator ID ||
|| **withStamps**
[`char`](../../data-types.md) | Include stamps and signatures. Possible values:
- `Y` — yes
- `N` — no
||
|| **providers**
[`object`](../../data-types.md) | Template data providers. 

Key and value are equal to the name of the data provider class ||
|| **users**
[`object`](../../data-types.md) | Access codes for the template. 

Key and value are equal to the access code ||
|| **isDeleted**
[`char`](../../data-types.md) | Template deletion flag. Possible values:
- `Y` — deleted
- `N` — not deleted
||
|| **sort**
[`string`](../../data-types.md) | Sort index ||
|| **createTime**
[`datetime`](../../data-types.md) | Template creation time ||
|| **updateTime**
[`datetime`](../../data-types.md) | Template last update time ||
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
|| `400` | `0` | Template not found | Template with the specified `id` not found ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to modify templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-template-add.md)
- [{#T}](./document-generator-template-update.md)
- [{#T}](./document-generator-template-list.md)
- [{#T}](./document-generator-template-delete.md)
- [{#T}](./document-generator-template-get-fields.md)