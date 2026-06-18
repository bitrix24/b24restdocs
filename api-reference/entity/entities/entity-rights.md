# Get or Modify Access Permissions for entity.rights

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method:
> - Any user can retrieve current permissions
> - A user with access level `X` (management) can modify permissions for the data storage

The `entity.rights` method retrieves the current set of access permissions for the application's data storage or modifies it.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Note on Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage. 

You can obtain the identifier using the [entity.get](./entity-get.md) method. ||
|| **ACCESS**
[`object`](../../data-types.md) | A new set of permissions in the format `{"access_code":"access_level"}`.

Examples of access codes:
- `U<id>` — user, for example `U1`
- `G<id>` — user group, for example `G2`
- `AU` — all authorized users

The method accepts standard access codes from Bitrix24. You can check the name of the code using the [access.name](../../common/system/access-name.md) method.

Supported levels:
- `R` — read
- `W` — write
- `X` — management

If a different level is provided, that permission entry will not be added.

When `ACCESS` is provided, the current user is forcibly granted the `X` permission (`U<id>`).

If the parameter is not provided, the method returns the current set of access permissions. ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

Example of modifying access permissions, where:
- `ENTITY` — identifier of the storage `dish`
- `ACCESS` — new set of permissions: `U1` with level `W` and `AU` with level `R`

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","ACCESS":{"U1":"W","AU":"R"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.rights
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EntityRightsResult = Record<string, string> | null

    try {
      const response = await $b24.actions.v2.call.make<EntityRightsResult>({
        method: 'entity.rights',
        params: {
          ENTITY: 'dish',
          ACCESS: {
            U1: 'W',
            AU: 'R',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Access rights:', result)
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
      async function setEntityRights() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'entity.rights',
            params: {
              ENTITY: 'dish',
              ACCESS: {
                U1: 'W',
                AU: 'R',
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
          console.info('Access rights:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setEntityRights)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'entity.rights',
                [
                    'ENTITY' => 'dish',
                    'ACCESS' => [
                        'U1' => 'W',
                        'AU' => 'R',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting entity rights: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.rights',
        {
            ENTITY: 'dish',
            ACCESS: {
                U1: 'W',
                AU: 'R',
            },
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'entity.rights',
        [
            'ENTITY' => 'dish',
            'ACCESS' => [
                'U1' => 'W',
                'AU' => 'R',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "U1": "W",
        "AU": "R",
        "U577": "X"
    },
    "time": {
        "start": 1774267885,
        "finish": 1774267885.803565,
        "duration": 0.8035650253295898,
        "processing": 0,
        "date_start": "2026-03-23T15:11:25+02:00",
        "date_finish": "2026-03-23T15:11:25+02:00",
        "operating_reset_at": 1774268485,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`result`](#result) | Root element of the response. Contains the current set of access permissions for the storage. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
|#

#### Type result {#result}

#|
|| **Name**
`type` | **Description** ||
||
[`object`](../../data-types.md) | Access permissions object in the format `{"access_code":"access_level"}`, where access level is `R`, `W`, or `X`. ||
||
`null` | Returned if the storage with the provided `ENTITY` is not found. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'ENTITY' is null or empty",
    "argument": "ENTITY"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | Parameter `ENTITY` is not provided or is empty after cleanup. ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is 13 characters. | The value of `ENTITY` is too long. ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to modify access permissions for the storage. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-add.md)
- [{#T}](./entity-update.md)
- [{#T}](./entity-get.md)
- [{#T}](./entity-delete.md)