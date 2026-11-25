# Edit Document Text note.document.content.edit

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and "Edit" permissions for the required document or document knowledge base

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.document.content.edit` method modifies document text using targeted replacements or replaces the entire Markdown content.

Before making replacements, retrieve the current text using [note.document.content.get](./note-document-content-get.md): the `old` values are compared against this exact text. Open editors receive the change as a co-author's edit. All replacements are applied atomically: if one is rejected, the document remains unchanged.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Document identifier ||
|| **edits***
[`array`](../../data-types.md) | Array of `1` to `50` replacements. [Object structure description](#edits).

Pass either `edits` or `markdown` ||
|| **markdown***
[`string`](../../data-types.md) | New full document text in Markdown, no more than 1 048 576 bytes.

Blocks from `opaqueBlocks` that the new text retains unchanged and in the same order remain as they are. Blocks missing from the new text are deleted. Such a block cannot be modified: the method returns `NOTE_CONTENT_BLOCK_UNSUPPORTED`.

Pass either `edits` or `markdown` ||
|#

### Edits Parameter {#edits}

#|
|| **Name**
`type` | **Description** ||
|| **old***
[`string`](../../data-types.md) | Fragment of the current Markdown to replace. It cannot be empty or consist only of whitespace.

Trailing spaces and tabs are ignored during comparison, and line breaks are normalized to `\n`.

If multiple matches are found without `replaceAll`, the method returns `NOTE_CONTENT_MATCH_AMBIGUOUS`. Replacement ranges must not overlap, including passing the same `old` in two replacements ||
|| **new***
[`string`](../../data-types.md) | New Markdown fragment ||
|| **replaceAll**
[`boolean`](../../data-types.md) | Replace all occurrences of `old`.

Default: `false` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.document.content.edit`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{"id":42,"edits":[{"old":"Old text","new":"New text","replaceAll":false}]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.document.content.edit
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{"id":42,"edits":[{"old":"Old text","new":"New text","replaceAll":false}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.document.content.edit
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v3.call.make({
        method: 'note.document.content.edit',
        params: {
          id: 42,
          edits: [{ old: 'Old text', new: 'New text', replaceAll: false }]
        },
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
      async function editContent() {
        try {
          const $b24 = await B24Js.initializeB24Frame()
          const response = await $b24.actions.v3.call.make({
            method: 'note.document.content.edit',
            params: {
              id: 42,
              edits: [{ old: 'Old text', new: 'New text', replaceAll: false }]
            },
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
      document.addEventListener('DOMContentLoaded', editContent)
    </script>
    ```

- Python

    There is no ready-made wrapper for this method in `b24pysdk` yet, and it is not included in the SDK's REST 3.0 method list. Add the method to `Config().api_v3_methods` and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "note.document.content.edit"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 42,
                "edits": [
                    {"old": "Old text", "new": "New text", "replaceAll": False},
                ],
            },
            prefer_version=3,
        )
        item = response["result"]["item"]
        print(item["changed"], item["markdown"])
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
            "changed": true,
            "markdown": "# Chapter 1\n\nNew text",
            "lossless": true,
            "opaqueBlocks": [],
            "warnings": []
        }
    },
    "time": {
        "start": 1795591533,
        "finish": 1795591533.411631,
        "duration": 0.41163110733032227,
        "processing": 0,
        "date_start": "2026-11-25T10:25:33+03:00",
        "date_finish": "2026-11-25T10:25:33+03:00",
        "operating_reset_at": 1795592100,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **item**
[`object`](../../data-types.md) | Document modification result. [Object structure description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Document identifier ||
|| **changed**
[`boolean`](../../data-types.md) | `true` if the document changed; `false` if the edit resulted in the same text and no write was performed ||
|| **markdown**
[`string`](../../data-types.md) | Current document text after the edit, including concurrent co-author changes ||
|| **lossless**
[`boolean`](../../data-types.md) | `true` if Markdown fully represents the document content ||
|| **opaqueBlocks**
[`array`](../../data-types.md) | Blocks that Markdown cannot fully represent. The field format is described in [note.document.content.get](./note-document-content-get.md#opaque-blocks) ||
|| **warnings**
[`array`](../../data-types.md) | Warnings about Markdown transformations when applying the edit. [Object structure description](#warnings) ||
|#

### Warnings Object {#warnings}

#|
|| **Name**
`type` | **Description** ||
|| **editIndex**
[`integer`](../../data-types.md) | Replacement index in the `edits` array, starting from `0`. `null` if the full `markdown` was passed ||
|| **code**
[`string`](../../data-types.md) | Warning code:
- `DEGRADED_TO_PARAGRAPH` — the block does not match the document schema and became a paragraph with its text
- `BLOCK_DROPPED` — the block does not match the document schema and contains no text, so it was skipped
- `INVALID_UTF8` — the text was not in UTF-8, and invalid bytes were replaced
- `PARTIALLY_PARSED` — part of the text was too long or complex to parse as Markdown and was retained as plain text
- `LINK_DROPPED` — the link to an invalid address was removed, and its text was retained
- `FILE_NOT_ACCESSIBLE` — the file from the `[[file fileId=…]]` token is unavailable to the user, and the token was retained as text ||
|| **message**
[`string`](../../data-types.md) | Description of the transformation in English ||
|#

## Error Handling

HTTP status: **409**

```json
{
    "error": {
        "code": "NOTE_CONTENT_MATCH_NOT_FOUND",
        "message": "The old fragment was not found in the article. Use note.document.content.get to get the current text and try the edit again.",
        "details": {
            "editIndex": 0
        }
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#|
|| **HTTP Status** | **Code** | **Description** ||
|| `400` | `NOTE_CONTENT_EDIT_INVALID` | The request is malformed:
- both `edits` and `markdown` were passed, or neither was passed
- `edits` is empty or contains more than 50 elements
- a replacement has an empty `old`, is missing `new`, has a non-Boolean `replaceAll`, contains an extra field, or the text is not in UTF-8
- the replacement Markdown does not match the document schema

The field with the error is passed in `details.field`, and the replacement index is passed in `details.editIndex` ||
|| `409` | `NOTE_CONTENT_MATCH_NOT_FOUND` | The `old` fragment was not found in the current Markdown. The replacement index is passed in `details.editIndex` ||
|| `409` | `NOTE_CONTENT_MATCH_AMBIGUOUS` | Multiple occurrences of `old` were found, and `replaceAll` was not passed. The number of matches is passed in `details.matches`, and the replacement index is passed in `details.editIndex` ||
|| `400` | `NOTE_CONTENT_EDITS_OVERLAP` | The ranges of multiple replacements overlap. The replacement indexes are passed in the `details.editIndexes` array ||
|| `409` | `NOTE_CONTENT_BLOCK_UNSUPPORTED` | A replacement or the new `markdown` modifies a block from `opaqueBlocks`, or an `old` boundary passes inside such a block. The block can be retained unchanged or deleted as a whole. The block index and type are passed in `details.blockIndex` and `details.nodeType` ||
|| `409` | `NOTE_CONTENT_FORMAT_UNSUPPORTED` | The document content is stored in a legacy format that the method does not support ||
|| `400` | `NOTE_MARKDOWN_TOO_LARGE` | The text exceeds 1 048 576 bytes: the `markdown` value, the total length of all `old` values, the total length of all `new` values, or the resulting document text. The limit is passed in `details.limit`, and the request field is passed in `details.field`: `markdown`, `edits[].old`, or `edits[].new`. No field is passed for the resulting text ||
|| `400` | `NOTE_CONTENT_MARKDOWN_TOO_COMPLEX` | The text is too complex to process. The reason is passed in `details.reason`:
- `PARSE_BUDGET` — Markdown parsing exceeds the allowed processing budget
- `MATCH_LIMIT` — replacements in a single request find more than 1000 matches; the limit is passed in `details.limit` ||
|| `400` | `NOTE_CONTENT_PATCH_TOO_LARGE` | The change is too large to send to open document editors: more than 921 600 bytes. Split the edit into several calls. The size and limit are passed in `details.size` and `details.limit` ||
|| `409` | `NOTE_CONTENT_TOO_LARGE` | The current document content on the server is too large for the method to process. Open the document in the editor and repeat the call ||
|| `409` | `NOTE_CONTENT_UNREADABLE` | The current state of the document could not be read ||
|| `409` | `NOTE_CONTENT_LOCKED` | The document is locked by another operation, and the lock could not be acquired within the allotted time ||
|| `400` | `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` | The document does not exist, is archived or in the Recycle Bin, cannot be viewed by the user, or a template or a knowledge base system document was specified ||
|| `403` | `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` | The user can view the document but does not have "Edit" permission ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-document-content-get.md)
- [{#T}](./note-document-update.md)
- [{#T}](../file/note-file-add.md)
