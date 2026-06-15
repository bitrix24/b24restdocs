# Get Role by ID documentgenerator.role.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: user with permission to modify document generator settings

The method `documentgenerator.role.get` returns information about the role and its access permissions.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Role identifier.

You can obtain the identifier after [creating a role](./document-generator-role-add.md) or by using the [get role list method](./document-generator-role-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.role.get
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.role.get
  ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type RoleGetResult = {
      role: {
        id: number
        name: string
        code: string
        permissions: {
          settings: {
            modify: string
          }
          templates: {
            modify: string
          }
          documents: {
            modify: string
            view: string
          }
        }
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<RoleGetResult>({
        method: 'documentgenerator.role.get',
        params: {
          id: 9,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.role.id, result.role.name, result.role.code, result.role.permissions)
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
      async function getRoleById() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'documentgenerator.role.get',
            params: {
              id: 9,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.role.id, result.role.name, result.role.code, result.role.permissions)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getRoleById)
    </script>
    ```

- PHP

  ```php
  try {
      $response = $b24Service->core->call(
          'documentgenerator.role.get',
          [
              'id' => 9,
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
      'documentgenerator.role.get',
      {
          id: 9
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
      'documentgenerator.role.get',
      [
          'id' => 9,
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
        "role": {
            "id": 9,
            "name": "Template Editors",
            "code": "DOCGEN_TEMPLATE_EDITORS",
            "permissions": {
                "settings": {
                    "modify": ""
                },
                "templates": {
                    "modify": "A"
                },
                "documents": {
                    "modify": "X",
                    "view": "X"
                }
            }
        }
    },
    "time": {
        "start": 1774014973,
        "finish": 1774014974.093322,
        "duration": 1.0933220386505127,
        "processing": 0,
        "date_start": "2026-03-20T16:56:13+02:00",
        "date_finish": "2026-03-20T16:56:14+02:00",
        "operating_reset_at": 1774015574,
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
|| **role**
[`object`](../../data-types.md) | Role data [(detailed description)](#role) ||
|#

#### Role Object {#role}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Role identifier ||
|| **name**
[`string`](../../data-types.md) | Role name ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the role ||
|| **permissions**
[`object`](../../data-types.md) | Role permissions [(detailed description)](#permissions) ||
|#

#### Permissions Object {#permissions}

#|
|| **Name**
`type` | **Description** ||
|| **settings**
[`object`](../../data-types.md) | Permissions for settings [(detailed description)](#permissions-settings) ||
|| **templates**
[`object`](../../data-types.md) | Permissions for templates [(detailed description)](#permissions-templates) ||
|| **documents**
[`object`](../../data-types.md) | Permissions for documents [(detailed description)](#permissions-documents) ||
|#

#### Settings Object {#permissions-settings}

#|
|| **Name**
`type` | **Description** ||
|| **modify**
[`string`](../../data-types.md) | Access level to settings. Possible values:
- `""` — no access
- `X` — full access
||
|#

#### Templates Object {#permissions-templates}

#|
|| **Name**
`type` | **Description** ||
|| **modify**
[`string`](../../data-types.md) | Access level to templates. Possible values:
- `""` — no access
- `A` — only own items
- `D` — own and department colleagues
- `X` — full access
 ||
|#

#### Documents Object {#permissions-documents}

#|
|| **Name**
`type` | **Description** ||
|| **modify**
[`string`](../../data-types.md) | Access level to modify documents. Possible values:
- `""` — no access
- `X` — full access
||
|| **view**
[`string`](../../data-types.md) | Access level to view documents. Possible values:
- `""` — no access
- `X` — full access
||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "0",
    "error_description": "You do not have permissions to modify settings"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\DocumentGenerator\Model\Role All parameters in the constructor must have real class type | Required parameter `id` not provided ||
|| `400` | `100` | Could not construct parameter {role} | Non-existent parameter `id` provided ||
|| `400` | `0` | You do not have permissions to modify settings | Insufficient permissions to modify document generator settings ||
|| `403` | `DOCGEN_ACCESS_ERROR` | Your plan does not support this operation | The plan does not support the function of permission delimitation for the document generator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-role-add.md)
- [{#T}](./document-generator-role-update.md)
- [{#T}](./document-generator-role-list.md)
- [{#T}](./document-generator-role-delete.md)
- [{#T}](./document-generator-role-fill-accesses.md)