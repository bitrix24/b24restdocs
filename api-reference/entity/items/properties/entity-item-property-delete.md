# Delete Property of Storage Elements entity.item.property.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access permission level `X` (management) on the data storage

The method `entity.item.property.delete` removes a property from the application's data storage elements.

{% note info "" %}

The method works only in the context of the [application](../../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](../../entities/entity-get.md) method ||
|| **PROPERTY**^*^
[`string`](../../../data-types.md) | Code of the property to be deleted.

You can obtain the property code using the [entity.item.property.get](./entity-item-property-get.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of deleting a property where:
- `ENTITY` — identifier of the storage `dish`
- `PROPERTY` — code of the property `new_prop`

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","PROPERTY":"new_prop","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.item.property.delete
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
        method: 'entity.item.property.delete',
        params: {
          ENTITY: 'dish',
          PROPERTY: 'new_prop',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Property deleted:', result)
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
      async function deleteEntityItemProperty() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'entity.item.property.delete',
            params: {
              ENTITY: 'dish',
              PROPERTY: 'new_prop',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Property deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteEntityItemProperty)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'entity.item.property.delete',
                [
                    'ENTITY' => 'dish',
                    'PROPERTY' => 'new_prop',
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
        echo 'Error deleting entity item property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.property.delete',
        {
            ENTITY: 'dish',
            PROPERTY: 'new_prop',
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
        'entity.item.property.delete',
        [
            'ENTITY' => 'dish',
            'PROPERTY' => 'new_prop',
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
        "start": 1774441734,
        "finish": 1774441734.535435,
        "duration": 0.5354349613189697,
        "processing": 0,
        "date_start": "2026-03-25T15:28:54+02:00",
        "date_finish": "2026-03-25T15:28:54+02:00",
        "operating_reset_at": 1774442334,
        "operating": 0.11034393310546875
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the property deletion (`true` — successful) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_PROPERTY_NOT_FOUND",
    "error_description": "Property not found"
}
```

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'ENTITY' is null or empty",
    "argument": "ENTITY"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter was not provided or is empty after cleanup ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The `ENTITY` value is too long ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | Storage with the provided `ENTITY` was not found ||
|| `ERROR_PROPERTY_NOT_FOUND` | Property not found | Property with the provided `PROPERTY` was not found ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to delete the property ||
|| `ACCESS_DENIED` | Access denied! Application context required | No application context (`clientId`) ||
|| `ERROR_CORE` | Internal error deleting entity property. Try deleting again. | Internal error while deleting the property ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-item-property-get.md)
- [{#T}](./entity-item-property-add.md)
- [{#T}](./entity-item-property-update.md)
- [{#T}](./index.md)