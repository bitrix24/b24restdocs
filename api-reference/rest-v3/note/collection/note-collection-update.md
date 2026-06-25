# Rename Knowledge Base note.collection.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and "Edit" permissions for the relevant knowledge base

The `note.collection.update` method changes the name of an existing knowledge base.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Knowledge base identifier.

The identifier can be obtained using the [note.collection.list](./note-collection-list.md) method. ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields that need to be changed. [Object structure description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | New knowledge base name.

The knowledge base name must not exceed 255 characters. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.collection.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":42,"fields":{"name":"Product documentation"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.collection.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":42,"fields":{"name":"Product documentation"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.collection.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CollectionUpdateResult = {
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
      const response = await $b24.actions.v3.call.make<CollectionUpdateResult>({
        method: 'note.collection.update',
        params: {
          id: 42,
          fields: {
            name: 'Product documentation',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Collection updated:', result.item.id, result.item.name)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function updateCollection() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.collection.update',
            params: {
              id: 42,
              fields: {
                name: 'Product documentation',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Collection updated:', result.item.id, result.item.name)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCollection)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.collection.update',
                [
                    'id' => 42,
                    'fields' => [
                        'name' => 'Product documentation',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating collection: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.collection.update',
        {
            id: 42,
            fields: {
                name: 'Product documentation'
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
        'note.collection.update',
        [
            'id' => 42,
            'fields' => [
                'name' => 'Product documentation'
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
            "updatedAt": "2026-04-21T09:15:30Z"
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
[`object`](../../../data-types.md) | Object with the result of the knowledge base update. ||
|| **item**
[`object`](../../../data-types.md) | Knowledge base object after the update. ||
|| **id**
[`integer`](../../../data-types.md) | Knowledge base identifier. ||
|| **name**
[`string`](../../../data-types.md) | Knowledge base name. ||
|| **position**
[`integer`](../../../data-types.md) | Knowledge base position in the general list. ||
|| **policyLevel**
[`string`](../../../data-types.md) | Base access policy of the knowledge base. ||
|| **createdBy**
[`integer`](../../../data-types.md) | Knowledge base author identifier. ||
|| **createdAt**
[`datetime`](../../../data-types.md) | Knowledge base creation date and time in UTC. ||
|| **updatedBy**
[`integer`](../../../data-types.md) | Last knowledge base editor identifier. ||
|| **updatedAt**
[`datetime`](../../../data-types.md) | Last knowledge base modification date and time in UTC. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "message": "Mandatory field `id` is missing",
                "field": "id"
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
|| `id`
`fields` | Required field `#FIELD#` is not specified. | Add the specified field to the request body ||
|| `id` | Field `#FIELD#` requires data type `#TYPE#` for this request. | Ensure the provided value is of the correct type ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Field `#FIELD#` requires data type `#TYPE#` for this request. | Ensure the provided value is of the correct type ||
|| `name` | Knowledge base name cannot be empty. | Pass a non-empty string in the `fields.name` field. ||
|| `name` | Knowledge base name must not exceed 255 characters. | Shorten the value of `fields.name`. ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied. | The user does not have access to the Knowledge Base module or the permissions to modify the knowledge base. ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Knowledge base not found. | Please check that the knowledge base exists and is accessible to the user. ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-collection-add.md)
- [{#T}](./note-collection-archive.md)
- [{#T}](./note-collection-delete.md)
- [{#T}](./index.md)