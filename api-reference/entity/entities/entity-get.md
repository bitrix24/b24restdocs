# Get Storage Parameters or List of Storages entity.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user upon application authorization

The `entity.get` method returns the parameters of the specified storage or a list of all storages of the application.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**
[`string`](../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

If the parameter is provided, the method returns data only for that storage.

Allowed characters are `a-z`, `A-Z`, `0-9`, `_` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of retrieving parameters for a specific storage, where `ENTITY` is the identifier `dish_v2`.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish_v2","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EntityGetResult = {
      ID: string
      IBLOCK_TYPE_ID: string
      ENTITY: string
      NAME: string
    }

    try {
      const response = await $b24.actions.v2.call.make<EntityGetResult>({
        method: 'entity.get',
        params: {
          ENTITY: 'dish_v2',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.ID, result.ENTITY, result.NAME)
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
      async function getEntity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'entity.get',
            params: {
              ENTITY: 'dish_v2',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.ID, result.ENTITY, result.NAME)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEntity)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'entity.get',
                [
                    'ENTITY' => 'dish_v2',
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
        echo 'Error getting entity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.get',
        {
            ENTITY: 'dish_v2',
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
        'entity.get',
        [
            'ENTITY' => 'dish_v2',
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
        "ID": "183",
        "IBLOCK_TYPE_ID": "rest_entity",
        "ENTITY": "dish_v2",
        "NAME": "Dishes v2"
    },
    "time": {
        "start": 1774270219,
        "finish": 1774270219.086362,
        "duration": 0.08636188507080078,
        "processing": 0,
        "date_start": "2026-03-23T15:50:19+01:00",
        "date_finish": "2026-03-23T15:50:19+01:00",
        "operating_reset_at": 1774270819,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`result`](#result) | Root element of the response. Contains the storage object or a list of storages ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type result {#result}

#|
|| **Name**
`type` | **Description** ||
|| `object`
[`entity`](#entity) | Returned if the `ENTITY` parameter is provided ||
|| [`entity[]`](#entity) | Returned if the `ENTITY` parameter is not provided ||
|#

#### Type entity {#entity}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Identifier of the storage ||
|| **IBLOCK_TYPE_ID**
[`string`](../../data-types.md) | Identifier of the storage type ||
|| **ENTITY**
[`string`](../../data-types.md) | Identifier of the storage provided by the application ||
|| **NAME**
[`string`](../../data-types.md) | Name of the storage ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ENTITY_NOT_FOUND",
    "error_description": "Entity not found"
}
```

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Entity code is too long. Max length is 13 characters.",
    "argument": ""
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter is provided but is empty after cleaning ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is 13 characters. | The `ENTITY` value is too long ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | Storage with the provided `ENTITY` not found ||
|| `ACCESS_DENIED` | Access denied! Application context required | No application context (`clientId`) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-add.md)
- [{#T}](./entity-update.md)
- [{#T}](./entity-delete.md)
- [{#T}](./entity-rights.md)