# Delete Data Store entity.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: a user with `X` (management) access permission on the data store

The `entity.delete` method removes the application's data store.

{% note info "" %}

This method only works in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Identifier of the application's data store. Use the value specified when creating the store.

You can obtain the identifier using the [entity.get](./entity-get.md) method ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of deleting a store where `ENTITY` is the identifier `dish_v2`.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish_v2","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'entity.delete',
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
        console.info('entity.delete result:', result)
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
      async function deleteEntity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'entity.delete',
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
          console.info('entity.delete result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteEntity)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'entity.delete',
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
        echo 'Error deleting entity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.delete',
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
        'entity.delete',
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
    "result": true,
    "time": {
        "start": 1774271103,
        "finish": 1774271103.40342,
        "duration": 0.40341997146606445,
        "processing": 0,
        "date_start": "2026-03-23T16:05:03+01:00",
        "date_finish": "2026-03-23T16:05:03+01:00",
        "operating_reset_at": 1774271703,
        "operating": 0.13090085983276367
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | The result of the method execution. Returns `true` for successful deletion ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ENTITY_NOT_FOUND",
    "error_description": "Entity not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter is not provided or is empty after cleanup ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The `ENTITY` value is too long ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | The store with the provided `ENTITY` was not found ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to delete the store ||
|| `ERROR_CORE` | Internal error deleting entity. Try deleting again. | Internal error while deleting the store ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-add.md)
- [{#T}](./entity-update.md)
- [{#T}](./entity-get.md)
- [{#T}](./entity-rights.md)