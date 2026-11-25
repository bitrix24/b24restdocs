# Retrieve Current Document Text note.document.content.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and "View" permission for the document's knowledge base

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.document.content.get` method retrieves the current document text in Markdown and the blocks that Markdown cannot fully represent.

Unlike [note.document.get](./note-document-get.md), this method reads the current state of the collaborative editor without lag. The `old` values passed to [note.document.content.edit](./note-document-content-edit.md) are compared against this exact text.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Document identifier.

The identifier can be obtained using the [note.document.tree.list](./note-document-tree-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.document.content.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{"id":42}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.document.content.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{"id":42,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.document.content.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v3.call.make({
        method: 'note.document.content.get',
        params: { id: 42 },
        requestId: Text.getUuidRfc4122()
      })
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info(response.getData()!.result)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getContent() {
        try {
          const $b24 = await B24Js.initializeB24Frame()
          const response = await $b24.actions.v3.call.make({
            method: 'note.document.content.get',
            params: { id: 42 },
            requestId: B24Js.Text.getUuidRfc4122()
          })
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }
          console.info(response.getData().result)
        } catch (error) {
          console.error(error)
        }
      }
      document.addEventListener('DOMContentLoaded', getContent)
    </script>
    ```

- Python

    There is no ready-made wrapper for this method in `b24pysdk` yet, and it is not included in the SDK's REST 3.0 method list. Add the method to `Config().api_v3_methods` and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "note.document.content.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 42,
            },
            prefer_version=3,
        )
        item = response["result"]["item"]
        print(item["markdown"], item["lossless"])
    except BitrixAPIError as error:
        print(f"code: {error.code}", f"message: {error.error.message}", sep="\n")
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    ```

- PHP

    The SDK does not yet support calls to the `/rest/api/` address. Use direct HTTP requests, for example, via `curl` or `fetch`.

- BX24.js

    The JS API does not yet support calls to the `/rest/api/` address. Use direct HTTP requests, for example, via `fetch`.

- PHP CRest

    CRest does not yet support calls to the `/rest/api/` address. Use direct HTTP requests, for example, via `curl`.

- Go

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request.

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 42,
            "markdown": "# Chapter 1\n\nDocument text",
            "lossless": true,
            "opaqueBlocks": []
        }
    },
    "time": {
        "start": 1795591363,
        "finish": 1795591363.867149,
        "duration": 0.8671491146087644,
        "processing": 0,
        "date_start": "2026-11-25T10:22:43+03:00",
        "date_finish": "2026-11-25T10:22:43+03:00",
        "operating_reset_at": 1795591920,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **item**
[`object`](../../data-types.md) | Current document content. [Object structure description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Document identifier ||
|| **markdown**
[`string`](../../data-types.md) | Current document text in Markdown ||
|| **lossless**
[`boolean`](../../data-types.md) | `true` if Markdown fully represents the document content; `false` if there are ambiguous blocks in `opaqueBlocks` ||
|| **opaqueBlocks**
[`array`](../../data-types.md) | Array of blocks that Markdown cannot fully represent. [Object structure description](#opaque-blocks) ||
|#

### OpaqueBlocks Object {#opaque-blocks}

Each array element contains the following fields.

#|
|| **Name**
`type` | **Description** ||
|| **index**
[`integer`](../../data-types.md) | Block position among the document's top-level blocks, starting from `0` ||
|| **nodeType**
[`string`](../../data-types.md) | Editor block type ||
|| **excerpt**
[`string`](../../data-types.md) | First 200 characters of the block's Markdown representation with whitespace collapsed ||
|| **reasons**
[`array`](../../data-types.md) | Reasons why the block cannot be represented in Markdown without loss.

For example: `NODE_OUTSIDE_CORE`, `LOSSY_ATTR`, `ROUNDTRIP_MISMATCH`.

Additional details may be specified after a colon ||
|#

## Error Handling

HTTP status: **409**

```json
{
    "error": {
        "code": "NOTE_CONTENT_FORMAT_UNSUPPORTED",
        "message": "The article content is stored in a legacy format that this method does not support. Use note.document.get to get the text."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#|
|| **HTTP Status** | **Code** | **Description** ||
|| `409` | `NOTE_CONTENT_FORMAT_UNSUPPORTED` | The document content is stored in a legacy format that the method does not support. Retrieve the text using [note.document.get](./note-document-get.md) ||
|| `409` | `NOTE_CONTENT_TOO_LARGE` | The current document content on the server is too large for the method to process. Open the document in the editor and repeat the call ||
|| `400` | `NOTE_CONTENT_MARKDOWN_TOO_COMPLEX` | Parsing the document content exceeds the allowed processing budget ||
|| `409` | `NOTE_CONTENT_UNREADABLE` | The current state of the document could not be read ||
|| `400` | `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` | The document does not exist, the user cannot view it, or a template or a knowledge base system document was specified ||
|#

The method retrieves the text of both archived documents and documents in the Recycle Bin if the user can view them.

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-document-content-edit.md)
- [{#T}](./note-document-get.md)
- [{#T}](./note-document-update.md)
