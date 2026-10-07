# Get a List of Knowledge Bases note.collection.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.collection.list` method returns a list of Knowledge bases available to the user.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **pagination**
[`object`](../../data-types.md) | Pagination object. [Object structure description](#pagination) ||
|#

### Parameter Pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Page size.

Allowed values: from `1` to `200`

Default: `50`.

The value is converted to an integer. Values above `200` are reduced to `200`; zero, negative, or nonnumeric values are replaced with `50` without a validation error ||
|| **afterCursor**
[`object`](../../data-types.md) | Next page cursor. Pass `result.nextCursor` from the previous response. [Object structure description](#aftercursor).

Default: first page. If the cursor does not contain both `position` and `id`, or is not an object, it is ignored without a validation error ||
|#

### Parameter afterCursor {#aftercursor}

#|
|| **Name**
`type` | **Description** ||
|| **position***
[`integer`](../../data-types.md) | The `position` field value of the last knowledge base from the previous page.

Required if `afterCursor` is specified ||
|| **id***
[`integer`](../../data-types.md) | The identifier of the last knowledge base from the previous page.

Required if `afterCursor` is specified ||
|#

Cursor fields are converted to integers. To avoid restarting pagination or skipping records, pass the cursor from the response unchanged. Keep the same `pagination.limit` in the next request. Stop when `result.nextCursor = null`.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

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

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    pagination = {
        "limit": 50,
        "afterCursor": {
            "position": 100,
            "id": 42,
        },
    }

    try:
        bitrix_response = client.note.collection.list(
            pagination=pagination,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "note.collection.list", b24.Params{
    	"pagination": b24.Params{
    		"limit": 50,
    		"afterCursor": b24.Params{
    			"position": 100,
    			"id":       42,
    		},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("note.collection.list: %w", err)
    }

    // The response shape is shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "nextCursor": null,
        "items": [
            {
                "id": 9,
                "name": "Knowledge Base 1",
                "position": 100,
                "policyLevel": "private",
                "accessLevel": "full",
                "isArchived": false,
                "createdBy": 1,
                "createdAt": "2026-06-23T22:01:03+03:00",
                "updatedBy": 1,
                "updatedAt": "2026-06-23T22:05:39+03:00",
                "markdownDescription": null
            },
            {
                "id": 7,
                "name": "Knowledge Base 2",
                "position": 100,
                "policyLevel": "private",
                "accessLevel": "full",
                "isArchived": false,
                "createdBy": 1,
                "createdAt": "2026-06-22T12:16:33+03:00",
                "updatedBy": 1,
                "updatedAt": "2026-06-22T12:16:33+03:00",
                "markdownDescription": null
            }
        ]
    },
    "time": {
        "start": 1791206039,
        "finish": 1791206039.885758,
        "duration": 0.8857579231262207,
        "processing": 0,
        "date_start": "2026-10-05T16:13:59+03:00",
        "date_finish": "2026-10-05T16:13:59+03:00",
        "operating_reset_at": 1791206639,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing a list of knowledge bases and a cursor ||
|| **result.items**
[`array`](../../data-types.md) | Array of knowledge base objects available to the user ||
|| **result.items[].id**
[`integer`](../../data-types.md) | Knowledge base identifier ||
|| **result.items[].name**
[`string`](../../data-types.md) | Knowledge base name ||
|| **result.items[].position**
[`integer`](../../data-types.md) | Position of the knowledge base in the list ||
|| **result.items[].policyLevel**
[`string`](../../data-types.md) | Knowledge base access policy code, such as `private` or `portal`. The current user's access level is returned separately in `accessLevel` ||
|| **result.items[].accessLevel**
[`string`](../../data-types.md) | Current user's access level code for the knowledge base. `full` in the example ||
|| **result.items[].isArchived**
[`boolean`](../../data-types.md) | `true` if the knowledge base is archived; `false` otherwise ||
|| **result.items[].createdBy**
[`integer`](../../data-types.md) | Knowledge base author identifier ||
|| **result.items[].updatedBy**
[`integer`](../../data-types.md) | Last knowledge base editor identifier ||
|| **result.items[].createdAt**
[`datetime`](../../data-types.md) or `null` | Creation date and time in ISO 8601 format with a timezone offset ||
|| **result.items[].updatedAt**
[`datetime`](../../data-types.md) or `null` | Last modification date and time in ISO 8601 format with a timezone offset ||
|| **result.items[].markdownDescription**
[`string`](../../data-types.md) or `null` | Additional Markdown description. `null` in the example ||
|| **result.nextCursor**
[`object`](../../data-types.md) or `null` | Cursor for the next page. `null` means there are no more pages ||
|| **result.nextCursor.position**
[`integer`](../../data-types.md) | Position of the last knowledge base on the page. Present when `nextCursor` is not `null` ||
|| **result.nextCursor.id**
[`integer`](../../data-types.md) | Identifier of the last knowledge base on the page. Present when `nextCursor` is not `null` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

Out-of-range `pagination.limit` values and an incomplete `pagination.afterCursor` do not cause validation errors: the values are normalized as described in [Method Parameters](#method-parameters).

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have access to the Knowledge Base module ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-collection-add.md)
- [{#T}](./note-collection-update.md)
- [{#T}](./index.md)
