# Files in Knowledge Base 2.0: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Files in Knowledge Base 2.0 are linked to documents and are used as Markdown attachments: images, videos, and regular files. The `note.file.*` methods help upload a file, retrieve its metadata, determine the field schema, and return a Markdown block for insertion into a document.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25971079/)

## Getting Started

1. Create a document using the [note.document.add](../document/note-document-add.md) method if you do not already have a page to which the file should be linked
2. Upload a file using the [note.file.add](./note-file-add.md) method and retain the returned `id` or immediately `assetMarkdown`
3. If you need to re-retrieve file metadata or a Markdown block by ID, call [note.file.get](./note-file-get.md)
4. Determine the field composition and types using the [note.file.field.list](./note-file-field-list.md) and [note.file.field.get](./note-file-field-get.md) methods if you are building a form, table, or validation on your side

## Limitations and Features

- The `note.file.add` method only uploads a file and links it to a document, but does not insert the attachment into the document text. After uploading, you must take the `assetMarkdown` from the method response or retrieve it via `note.file.get`, and then update the page
- The file size limit is taken from the `main.max_file_size` setting, and if it is not specified, a limit is used `25 MiB (25 * 1024 * 1024 bytes)`. Therefore, the allowed size may vary across different Bitrix24 instances
- Only specific file extensions are allowed. If an extension is not in the list of allowed extensions, the file will not be uploaded

## Connection with Other Objects

**Documents.** A file always belongs to a specific page rather than the Knowledge base as a whole. Therefore, before uploading a file, you must first create a document or select an existing document where the attachment should appear. The scenario consists of three steps:
- upload a file using the [note.file.add](./note-file-add.md) method
- take the `assetMarkdown` block from the `note.file.add` response or retrieve it using the [note.file.get](./note-file-get.md) method
- insert this block into `markdown` and update the document using the [note.document.update](../document/note-document-update.md) method

## Overview of Methods {#all-methods}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [note.file.add](./note-file-add.md) | Uploads a file to a document ||
|| [note.file.get](./note-file-get.md) | Returns the document file data and a Markdown block for insertion into the document ||
|| [note.file.field.get](./note-file-field-get.md) | Returns the description of a document file field ||
|| [note.file.field.list](./note-file-field-list.md) | Returns a list of document file fields ||
|#

## Continue Learning

- [{#T}](../document/index.md)
- [{#T}](../collection/index.md)
- [{#T}](../index.md)