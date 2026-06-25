# Get a List of Knowledge Bases note.collection.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module

The `note.collection.list` method returns a list of Knowledge bases available to the user.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object. [Object structure description](#pagination) ||
|#

### Parameter Pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Page size.

Allowed values: from `1` to `200`

Default: `50` ||
|| **afterCursor**
[`object`](../../../data-types.md) | Next page cursor. Pass the `nextCursor` value from the previous response. [Object structure description](#aftercursor) ||
|#

### Parameter afterCursor {#aftercursor}

#|
|| **Name**
`type` | **Description** ||
|| **position***
[`integer`](../../../data-types.md) | The `position` field value of the last knowledge base from the previous page.

Required if `afterCursor` is specified ||
|| **id***
[`integer`](../../../data-types.md) | The identifier of the last knowledge base from the previous page.

Required if `afterCursor` is specified ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.collection.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"pagination":{"limit":50,"afterCursor":{"position":100,"id":42}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.collection.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"pagination":{"limit":50,"afterCursor":{"position":100,"id":42}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.collection.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CollectionListResult = {
      items: Array<{
        id: number
        name: string
        position: number
        policyLevel: string
        createdBy: number
        updatedBy: number
        createdAt: ISODate
        updatedAt: ISODate
      }>
      nextCursor: {
        position: number
        id: number
      } | null
    }

    try {
      const response = await $b24.actions.v3.call.make<CollectionListResult>({
        method: 'note.collection.list',
        params: {
          pagination: {
            limit: 50,
            afterCursor: {
              position: 100,
              id: 42,
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Collections:', result.items.length, result.nextCursor)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listCollections() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.collection.list',
            params: {
              pagination: {
                limit: 50,
                afterCursor: {
                  position: 100,
                  id: 42,
                },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Collections:', result.items.length, result.nextCursor)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listCollections)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.collection.list',
                [
                    'pagination' => [
                        'limit' => 50,
                        'afterCursor' => [
                            'position' => 100,
                            'id' => 42,
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing collections: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.collection.list',
        {
            pagination: {
                limit: 50,
                afterCursor: {
                    position: 100,
                    id: 42
                }
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
        'note.collection.list',
        [
            'pagination' => [
                'limit' => 50,
                'afterCursor' => [
                    'position' => 100,
                    'id' => 42,
                ],
            ],
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
        "items": [
            {
                "id": 1,
                "name": "Product documentation",
                "position": 100,
                "policyLevel": "view",
                "createdBy": 1,
                "updatedBy": 1,
                "createdAt": "2026-04-20T12:00:00Z",
                "updatedAt": "2026-04-21T09:15:30Z"
            }
        ],
        "nextCursor": {
            "position": 100,
            "id": 1
        }
    },
    "time": {
        "start": 1780639200,
        "finish": 1780639200.224321,
        "duration": 0.2243211269378662,
        "processing": 0.18721413612365723,
        "date_start": "2026-06-19T10:00:00+03:00",
        "date_finish": "2026-06-19T10:00:00+03:00",
        "operating_reset_at": 1780639800,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object containing a list of knowledge bases ||
|| **items**
[`array`](../../../data-types.md) | List of knowledge bases available to the user ||
|| **items[]**
[`object`](../../../data-types.md) | Knowledge base object ||
|| **id**
[`integer`](../../../data-types.md) | Knowledge base identifier ||
|| **name**
[`string`](../../../data-types.md) | Knowledge base name ||
|| **position**
[`integer`](../../../data-types.md) | Position of the knowledge base in the overall list ||
|| **policyLevel**
[`string`](../../../data-types.md) | Base access policy of the knowledge base.

Possible values:

- `none` — no access
- `view` — view
- `manage` — edit
- `moderate` — administration ||
|| **createdBy**
[`integer`](../../../data-types.md) | Knowledge base author identifier ||
|| **updatedBy**
[`integer`](../../../data-types.md) | Last knowledge base editor identifier ||
|| **createdAt**
[`datetime`](../../../data-types.md) | Knowledge base creation date and time in UTC ||
|| **updatedAt**
[`datetime`](../../../data-types.md) | Knowledge base last modified date and time in UTC ||
|| **nextCursor**
[`object`](../../../data-types.md) | Next page cursor or `null` if there are no more pages ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION",
        "message": "Access denied"
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have access to the Knowledge Base module ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-collection-add.md)
- [{#T}](./note-collection-update.md)
- [{#T}](./index.md)