# Create a Knowledge Base note.collection.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and the "Create Knowledge Bases" permission

The `note.collection.add` method creates a new Knowledge base and returns its object.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields of the new knowledge base. [Object structure description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Knowledge base name.

The knowledge base name must not exceed 255 characters ||
|| **position**
[`integer`](../../../data-types.md) | Knowledge base position in the general list.

Default: `0` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.collection.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Product documentation","position":100}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.collection.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Product documentation","position":100},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.collection.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CollectionAddResult = {
      item: {
        id: number
        name: string
        position: number
        policyLevel: string
        createdBy: number
        updatedBy: number
        createdAt: string
        updatedAt: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<CollectionAddResult>({
        method: 'note.collection.add',
        params: {
          fields: {
            name: 'Product documentation',
            position: 100,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Collection created:', result.item.id, result.item.name)
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
      async function addCollection() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.collection.add',
            params: {
              fields: {
                name: 'Product documentation',
                position: 100,
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
          console.info('Collection created:', result.item.id, result.item.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCollection)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.collection.add',
                [
                    'fields' => [
                        'name' => 'Product documentation',
                        'position' => 100,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating collection: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.collection.add',
        {
            fields: {
                name: 'Product documentation',
                position: 100
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'note.collection.add',
        [
            'fields' => [
                'name' => 'Product documentation',
                'position' => 100
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 42,
            "name": "Product documentation",
            "position": 100,
            "policyLevel": "view",
            "createdBy": 1,
            "createdAt": "2026-04-20T12:00:00Z",
            "updatedBy": 1,
            "updatedAt": "2026-04-20T12:00:00Z"
        }
    },
    "time": {
        "start": 1780388120,
        "finish": 1780388120.245321,
        "duration": 0.24532103538513184,
        "processing": 0.1812450885772705,
        "date_start": "2026-06-16T11:15:20+03:00",
        "date_finish": "2026-06-16T11:15:20+03:00",
        "operating_reset_at": 1780388720,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the result of knowledge base creation ||
|| **item**
[`object`](../../../data-types.md) | Created knowledge base object ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the created knowledge base ||
|| **name**
[`string`](../../../data-types.md) | Knowledge base name ||
|| **position**
[`integer`](../../../data-types.md) | Knowledge base position in the general list ||
|| **policyLevel**
[`string`](../../../data-types.md) | Base access policy of the knowledge base ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the knowledge base author ||
|| **createdAt**
[`datetime`](../../../data-types.md) | Date and time of knowledge base creation in UTC ||
|| **updatedBy**
[`integer`](../../../data-types.md) | Identifier of the last knowledge base editor ||
|| **updatedAt**
[`datetime`](../../../data-types.md) | Date and time of the last knowledge base change in UTC ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Object validation error",
        "validation": [
            {
                "message": "Mandatory field \"name\" is not filled",
                "field": "name"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Mandatory field `fields` is not specified | Add object `fields` to the request body ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Mandatory field `name` is not filled | Add `fields.name` to the request body ||
|| `name`
`position` | Field `#FIELD#` requires data type `#TYPE#` for such a request | Ensure the provided value is of the correct type ||
|| `name` | Knowledge base name cannot be empty | Pass a non-empty string in the `fields.name` field ||
|| `name` | Knowledge base name must not exceed 255 characters | Shorten the value of `fields.name` ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have access to the Knowledge Base module or the permissions to create knowledge bases ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-collection-update.md)
- [{#T}](./note-collection-archive.md)
- [{#T}](./note-collection-delete.md)
- [{#T}](./index.md)