# Restore Knowledge Base from Recycle Bin note.collection.restore

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and "Administration" permissions for the relevant knowledge base

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.collection.restore` method restores a knowledge base from the Recycle Bin, along with the documents that were deleted together with it.

{% note info "" %}

Documents moved to the Recycle Bin before the knowledge base remain in the Recycle Bin. Restore them using the [note.document.restore](../document/note-document-restore.md) method.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the knowledge base in the Recycle Bin.

The identifier can be obtained using the [note.collection.list](./note-collection-list.md) method with the `[["state","=","trashed"]]` filter.

If the knowledge base was archived before it was deleted, it remains archived after the restore. Restore it from the archive using the [note.collection.unarchive](./note-collection-unarchive.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.collection.restore`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":49}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.collection.restore
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":49,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.collection.restore
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CollectionRestoreResult = {
      result: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<CollectionRestoreResult>({
        method: 'note.collection.restore',
        params: {
          id: 49,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Collection restored:', result.result)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function restoreCollection() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.collection.restore',
            params: {
              id: 49,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Collection restored:', result.result)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', restoreCollection)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.note.collection.restore(
            bitrix_id=49,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.collection.restore',
                [
                    'id' => 49,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'note.collection.restore',
        {
            id: 49,
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'note.collection.restore',
        [
            'id' => 49,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "note.collection.restore", b24.Params{
    	"id": 49,
    })
    if err != nil {
    	return fmt.Errorf("note.collection.restore: %w", err)
    }

    var item struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true
    },
    "time": {
        "start": 1792586277,
        "finish": 1792586277.28804,
        "duration": 0.2880399227142334,
        "processing": 0,
        "date_start": "2026-10-21T15:37:57+03:00",
        "date_finish": "2026-10-21T15:37:57+03:00",
        "operating_reset_at": 1792586820,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the operation result ||
|| **result.result**
[`boolean`](../../data-types.md) | Value of `true` if the knowledge base was restored. A repeated call for a knowledge base that has already been restored also returns `true` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Required field \"id\" is missing.",
                "field": "id"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field "id" is missing | Add `id` to the request body ||
|| `id` | This request requires data type "int" in field "id" | Pass the identifier as a number ||
|#

#### Restore Errors

#|
|| **Code** | **HTTP status** | **Error description** | **How to Fix** ||
|| `COLLECTION_RESTORE_IN_PROGRESS` | 409 | This knowledge base is already being restored. Try again later | Another restore of this knowledge base is still in progress. Repeat the request later ||
|| `COLLECTION_RETENTION_EXPIRED` | 410 | The retention period in the Recycle Bin has expired. This knowledge base can no longer be restored | The knowledge base cannot be restored ||
|| `COLLECTION_PURGE_STARTED` | 410 | Permanent deletion of this knowledge base has started. It can no longer be restored | The knowledge base cannot be restored ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have access to the Knowledge Base module or "Administration" permissions for the knowledge base ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID #ID# was not found | The knowledge base does not exist or is not accessible to the user. A knowledge base in the Recycle Bin is visible only to users with "Administration" permissions for it ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-collection-delete.md)
- [{#T}](./note-collection-unarchive.md)
- [{#T}](../document/note-document-restore.md)
- [{#T}](./index.md)
