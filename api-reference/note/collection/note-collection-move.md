# Change Knowledge Base Position note.collection.move

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and the "Create Knowledge Bases" permission

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.collection.move` method moves a knowledge base to a different position in the list of knowledge bases.

{% note info "" %}

The order of knowledge bases is shared across the entire Bitrix24: all employees see the change in the left menu.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Knowledge base identifier.

The identifier can be obtained using the [note.collection.list](./note-collection-list.md) method.

A knowledge base that is archived or in the Recycle Bin cannot be moved ||
|| **position**
[`integer`](../../data-types.md) | Sort weight of the knowledge base. Knowledge bases with a higher weight appear higher in the list. To place a knowledge base between two others, pass a number between their `position` values.

The retained weight is recalculated based on the neighboring knowledge bases and may differ from the value passed. The actual value is returned in the response.

Default: the knowledge base is placed last in the list ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.collection.move`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":47,"position":500}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.collection.move
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":47,"position":500,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.collection.move
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CollectionMoveResult = {
      item: {
        id: number
        name: string
        position: number
        policyLevel: string
        accessLevel: string
        state: 'live' | 'archived' | 'trashed'
        isArchived: boolean
        createdBy: number
        createdAt: ISODate
        updatedBy: number
        updatedAt: ISODate
        markdownDescription: string | null
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<CollectionMoveResult>({
        method: 'note.collection.move',
        params: {
          id: 47,
          position: 500,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Collection position:', result.item.position)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function moveCollection() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.collection.move',
            params: {
              id: 47,
              position: 500,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Collection position:', result.item.position)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', moveCollection)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.note.collection.move(
            bitrix_id=47,
            position=500,
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
                'note.collection.move',
                [
                    'id' => 47,
                    'position' => 500,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'note.collection.move',
        {
            id: 47,
            position: 500,
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
        'note.collection.move',
        [
            'id' => 47,
            'position' => 500,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "note.collection.move", b24.Params{
    	"id":       47,
    	"position": 500,
    })
    if err != nil {
    	return fmt.Errorf("note.collection.move: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID       b24.ID `json:"id"`
    	Name     string `json:"name"`
    	Position int    `json:"position"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Position)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 47,
            "name": "Policies",
            "position": 550,
            "policyLevel": "private",
            "accessLevel": "full",
            "state": "live",
            "isArchived": false,
            "createdBy": 1,
            "createdAt": "2026-10-21T15:29:56+03:00",
            "updatedBy": 1,
            "updatedAt": "2026-10-21T15:46:14+03:00",
            "markdownDescription": null
        }
    },
    "time": {
        "start": 1792586774,
        "finish": 1792586774.535677,
        "duration": 0.5356769561767578,
        "processing": 0,
        "date_start": "2026-10-21T15:46:14+03:00",
        "date_finish": "2026-10-21T15:46:14+03:00",
        "operating_reset_at": 1792587360,
        "operating": 0
    }
}
```

In this example, `position` = `500` was passed, but the retained weight is `550`: the value was recalculated based on the neighboring knowledge bases with weights `1000` and `100`.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the knowledge base data ||
|| **result.item**
[`object`](../../data-types.md) | Knowledge base as retained after the move ||
|| **result.item.id**
[`integer`](../../data-types.md) | Knowledge base identifier ||
|| **result.item.name**
[`string`](../../data-types.md) | Knowledge base name ||
|| **result.item.position**
[`integer`](../../data-types.md) | Retained sort weight of the knowledge base ||
|| **result.item.policyLevel**
[`string`](../../data-types.md) | Knowledge base access policy code, such as `private` or `portal` ||
|| **result.item.accessLevel**
[`string`](../../data-types.md) | Current user's access level code for the knowledge base, such as `full` ||
|| **result.item.state**
[`string`](../../data-types.md) | Knowledge base state: `live` — active, `archived` — archived, `trashed` — in the Recycle Bin ||
|| **result.item.isArchived**
[`boolean`](../../data-types.md) | `true` — the knowledge base is archived, `false` — not archived ||
|| **result.item.createdBy**
[`integer`](../../data-types.md) | Identifier of the knowledge base author ||
|| **result.item.createdAt**
[`datetime`](../../data-types.md) | Creation date and time in ISO 8601 format with a timezone offset ||
|| **result.item.updatedBy**
[`integer`](../../data-types.md) | Identifier of the last knowledge base editor ||
|| **result.item.updatedAt**
[`datetime`](../../data-types.md) | Last modification date and time in ISO 8601 format with a timezone offset ||
|| **result.item.markdownDescription**
[`string`](../../data-types.md) or `null` | Knowledge base description in Markdown ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **409**

```json
{
    "error": {
        "code": "NOTE_COLLECTION_IN_RECYCLE_BIN",
        "message": "This knowledge base is in the Recycle Bin. Restore it to make changes."
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

#### Knowledge Base in Recycle Bin Error

Error Code: `NOTE_COLLECTION_IN_RECYCLE_BIN`

HTTP status: **409**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | This knowledge base is in the Recycle Bin. Restore it to make changes | Restore the knowledge base using the [note.collection.restore](./note-collection-restore.md) method ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have access to the Knowledge Base module or the "Create Knowledge Bases" permission ||
|| `id` | Access denied | The knowledge base is archived. Restore it from the archive using the [note.collection.unarchive](./note-collection-unarchive.md) method ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID #ID# was not found | The knowledge base does not exist or is not accessible to the user ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-collection-list.md)
- [{#T}](./note-collection-update.md)
- [{#T}](../document/note-document-move.md)
- [{#T}](./index.md)
