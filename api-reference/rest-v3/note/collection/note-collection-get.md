# Get Knowledge Base note.collection.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and "View" permission for the required knowledge base

The `note.collection.get` method retrieves a knowledge base by identifier.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Knowledge base identifier.

The identifier can be obtained using the [note.collection.list](./note-collection-list.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.collection.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":42}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.collection.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":42,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.collection.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CollectionGetResult = {
      item: {
        id: number
        name: string
        position: number
        policyLevel: string
        createdBy: number
        updatedBy: number
        createdAt: ISODate
        updatedAt: ISODate
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<CollectionGetResult>({
        method: 'note.collection.get',
        params: {
          id: 42,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Collection name:', result.item.name)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getCollection() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.collection.get',
            params: {
              id: 42,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Collection name:', result.item.name)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCollection)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.collection.get',
                [
                    'id' => 42,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting collection: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.collection.get',
        {
            id: 42
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
        'note.collection.get',
        [
            'id' => 42
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
            "policyLevel": "manage",
            "createdBy": 1,
            "createdAt": "2026-04-20T12:00:00Z",
            "updatedBy": 1,
            "updatedAt": "2026-04-21T09:15:30Z"
        }
    },
    "time": {
        "start": 1780640400,
        "finish": 1780640400.215441,
        "duration": 0.21544098854064941,
        "processing": 0.1772141456604004,
        "date_start": "2026-06-19T10:20:00+03:00",
        "date_finish": "2026-06-19T10:20:00+03:00",
        "operating_reset_at": 1780641000,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Knowledge base data object ||
|| **item**
[`object`](../../../data-types.md) | Knowledge base object ||
|| **id**
[`integer`](../../../data-types.md) | Knowledge base identifier ||
|| **name**
[`string`](../../../data-types.md) | Knowledge base name ||
|| **position**
[`integer`](../../../data-types.md) | Knowledge base position in the general list ||
|| **policyLevel**
[`string`](../../../data-types.md) | Basic knowledge base access policy.

Possible values:

- `none` — no access
- `view` — view
- `manage` — edit
- `moderate` — administration ||
|| **createdBy**
[`integer`](../../../data-types.md) | Knowledge base author identifier ||
|| **createdAt**
[`datetime`](../../../data-types.md) | Knowledge base creation date and time in UTC ||
|| **updatedBy**
[`integer`](../../../data-types.md) | Last knowledge base editor identifier ||
|| **updatedAt**
[`datetime`](../../../data-types.md) | Knowledge base last modification date and time in UTC ||
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
|| `id` | Required field `id` is not specified | Add `id` to the request body ||
|| `id` | Field `id` requires data type `#TYPE#` for such a request | Ensure the provided value is of the correct type ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Record with ID = `#ID#` not found | Please check that the knowledge base exists and the user has permission to view it ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-collection-list.md)
- [{#T}](./note-collection-update.md)
- [{#T}](./index.md)