# Get a List of Document Files note.file.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and "View" permissions for the document

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.file.list` method returns the files attached to a document, page by page, along with their attachment dates.

{% note info "" %}

A file uploaded with the [note.file.add](./note-file-add.md) method and not inserted into the document text is eventually deleted by background cleanup. The period is counted from the `createdAt` date.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **documentId***
[`integer`](../../data-types.md) | Document identifier.

The identifier can be obtained using the [note.document.list](../document/note-document-list.md) method.

The list is also available for an archived document or a document in the Recycle Bin, provided the user can open it ||
|| **pagination**
[`object`](../../data-types.md) | Pagination object. [Object structure description](#pagination).

Omit this parameter for the first page ||
|#

### Parameter Pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of files per page.

Allowed values: from `1` to `200`. Values above `200` are reduced to `200`; zero and negative values are replaced with `50`. A nonnumeric value causes a validation error.

Default: `50` ||
|| **afterCursor**
[`object`](../../data-types.md) | Next page cursor. Pass the `result.nextCursor` value from the previous response unchanged.

The cursor structure is internal and may change. Do not build the cursor manually ||
|#

To retrieve all files of a document, call the method without `pagination.afterCursor`, then pass `result.nextCursor` from each response to `pagination.afterCursor` until `result.nextCursor` becomes `null`.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.file.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentId":131,"pagination":{"limit":1}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.file.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentId":131,"pagination":{"limit":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.file.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FileListResult = {
      items: Array<{
        id: number
        documentId: number
        name: string
        size: number
        mimeType: string
        assetType: 'image' | 'video' | 'file'
        assetMarkdown: string
        createdAt: ISODate
      }>
      nextCursor: Record<string, unknown> | null
    }

    try {
      const response = await $b24.actions.v3.call.make<FileListResult>({
        method: 'note.file.list',
        params: {
          documentId: 131,
          pagination: {
            limit: 1,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Files:', result.items.length, result.nextCursor)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listFiles() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.file.list',
            params: {
              documentId: 131,
              pagination: {
                limit: 1,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Files:', result.items.length, result.nextCursor)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listFiles)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.note.file.list(
            document_id=131,
            pagination={
                "limit": 1,
            },
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
                'note.file.list',
                [
                    'documentId' => 131,
                    'pagination' => [
                        'limit' => 1,
                    ],
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
        'note.file.list',
        {
            documentId: 131,
            pagination: {
                limit: 1,
            },
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
        'note.file.list',
        [
            'documentId' => 131,
            'pagination' => [
                'limit' => 1,
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
    res, err := client.Core().Call(ctx, "note.file.list", b24.Params{
    	"documentId": 131,
    	"pagination": b24.Params{
    		"limit": 1,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("note.file.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID            int    `json:"id"`
    		Name          string `json:"name"`
    		AssetMarkdown string `json:"assetMarkdown"`
    		CreatedAt     string `json:"createdAt"`
    	} `json:"items"`
    	// Pass the cursor to the next request unchanged.
    	NextCursor json.RawMessage `json:"nextCursor"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Name, item.CreatedAt)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "nextCursor": {
            "id": 39387
        },
        "items": [
            {
                "id": 39387,
                "documentId": 131,
                "name": "diagram.png",
                "size": 70,
                "mimeType": "image/png",
                "assetType": "image",
                "assetMarkdown": "[[image fileId=39387]]",
                "createdAt": "2026-10-21T15:38:14+03:00"
            }
        ]
    },
    "time": {
        "start": 1792586295,
        "finish": 1792586295.867693,
        "duration": 0.8676929473876953,
        "processing": 0,
        "date_start": "2026-10-21T15:38:15+03:00",
        "date_finish": "2026-10-21T15:38:15+03:00",
        "operating_reset_at": 1792586880,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the list of files and the cursor ||
|| **result.items**
[`array`](../../data-types.md) | Array of files on the page, sorted by identifier in ascending order ||
|| **result.items[].id**
[`integer`](../../data-types.md) | File identifier ||
|| **result.items[].documentId**
[`integer`](../../data-types.md) | Document identifier ||
|| **result.items[].name**
[`string`](../../data-types.md) | File name ||
|| **result.items[].size**
[`integer`](../../data-types.md) | File size in bytes ||
|| **result.items[].mimeType**
[`string`](../../data-types.md) | File MIME type ||
|| **result.items[].assetType**
[`string`](../../data-types.md) | Attachment type.

Possible values:

- `image` — image
- `video` — video
- `file` — other file ||
|| **result.items[].assetMarkdown**
[`string`](../../data-types.md) | Markdown block for inserting the file into the document text. To check whether the file is inserted into the text, search for this block in the document's `markdown` returned by [note.document.get](../document/note-document-get.md) ||
|| **result.items[].createdAt**
[`datetime`](../../data-types.md) | Date and time the file was attached to the document, in ISO 8601 format with a time zone. The background cleanup period for files not inserted into the text is counted from this date ||
|| **result.nextCursor**
[`object`](../../data-types.md) or `null` | Next page cursor. Pass it to `pagination.afterCursor` unchanged. `null` means there are no more pages ||
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
                "message": "Required field \"documentId\" is missing.",
                "field": "documentId"
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
|| `documentId` | Required field "documentId" is missing | Add `documentId` to the request body ||
|| `documentId` | This request requires data type "int" in field "documentId" | Pass the identifier as a number ||
|| `pagination.limit` | must be a number | Pass a number ||
|| `afterCursor.id` | must be a number | Pass the cursor from `result.nextCursor` unchanged ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have access to the Knowledge Base module ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `documentId` | Entity with ID #ID# was not found | The document does not exist or is not accessible to the user ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-file-add.md)
- [{#T}](./note-file-get.md)
- [{#T}](../document/note-document-get.md)
- [{#T}](./index.md)
