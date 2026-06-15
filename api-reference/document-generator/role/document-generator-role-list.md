# Get a List of Roles documentgenerator.role.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator settings

The method `documentgenerator.role.list` returns a list of roles without detailing permissions.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to control pagination.

The page size for results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N — 1) * 50`, where `N` is the desired page number ||
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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.role.list
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.role.list
  ```

- JS (TS)

  ```ts
  // This snippet is an ES module: top-level await requires type="module" or a bundler.
  // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
  import { Text } from '@bitrix24/b24jssdk'
  import type { B24Frame } from '@bitrix24/b24jssdk'

  declare const $b24: B24Frame

  type RoleItem = {
    id: number
    name: string
    code: string
  }

  // Shape of the payload returned in result (match the "response handling" section of the page)
  type RoleListResult = {
    roles: RoleItem[]
  }

  try {
    // documentgenerator.role.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    const response = await $b24.actions.v2.call.make<RoleListResult>({
      method: 'documentgenerator.role.list',
      params: {
        start: 0,
      },
      requestId: Text.getUuidRfc4122()
    })

    // The payload is available only on a successful response
    if (!response.isSuccess) {
      console.error(response.getErrorMessages().join('; '))
    } else {
      const result = response.getData()!.result
      console.info(result.roles.length, result.roles)
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
    async function listRoles() {
      try {
        // Initialize the SDK inside a Bitrix24 frame
        const $b24 = await B24Js.initializeB24Frame()

        // documentgenerator.role.list returns a single page (max 50 records). For the whole result set
        // use a list helper: $b24.actions.v2.callList.make() returns every record as one
        // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
        // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
        // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
        const response = await $b24.actions.v2.call.make({
          method: 'documentgenerator.role.list',
          params: {
            start: 0,
          },
          requestId: B24Js.Text.getUuidRfc4122()
        })

        // The payload is available only on a successful response
        if (!response.isSuccess) {
          console.error(response.getErrorMessages().join('; '))
          return
        }

        const result = response.getData().result
        console.info(result.roles.length, result.roles)
      } catch (error) {
        // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
        console.error(error)
      }
    }

    document.addEventListener('DOMContentLoaded', listRoles)
  </script>
  ```

- PHP

  ```php
  try {
      $response = $b24Service->core->call(
          'documentgenerator.role.list',
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
      'documentgenerator.role.list',
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
      'documentgenerator.role.list',
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
        "roles": [
            {
                "id": 1,
                "name": "Administrator",
                "code": "ADMIN"
            },
            {
                "id": 3,
                "name": "Manager",
                "code": "MANAGER"
            },
            {
                "id": 9,
                "name": "Template Editors",
                "code": "DOCGEN_TEMPLATE_EDITORS"
            }
        ]
    },
    "total": 3,
    "time": {
        "start": 1774015085,
        "finish": 1774015085.612552,
        "duration": 0.6125519275665283,
        "processing": 0,
        "date_start": "2026-03-20T16:58:05+01:00",
        "date_finish": "2026-03-20T16:58:05+01:00",
        "operating_reset_at": 1774015685,
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
|| **total**
[`integer`](../../data-types.md) | Number of roles in the current selection ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **roles**
[`array`](../../data-types.md) | List of roles [(detailed description)](#roles) ||
|#

#### Role Array Element {#roles}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Role identifier ||
|| **name**
[`string`](../../data-types.md) | Role name ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the role ||
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
|| `400` | `0` | You do not have permissions to modify settings | Insufficient rights to modify document generator settings ||
|| `403` | `DOCGEN_ACCESS_ERROR` | Your plan does not support this operation | The plan does not allow for permission segregation for the document generator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-role-add.md)
- [{#T}](./document-generator-role-update.md)
- [{#T}](./document-generator-role-get.md)
- [{#T}](./document-generator-role-delete.md)
- [{#T}](./document-generator-role-fill-accesses.md)