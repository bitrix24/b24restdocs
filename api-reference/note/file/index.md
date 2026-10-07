# Files in Knowledge Base 2.0: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Files in Knowledge Base 2.0 are linked to documents and are used as Markdown attachments: images, videos, and regular files. The `note.file.*` methods help upload a file, retrieve its metadata, determine the field schema, and return a Markdown block for insertion into a document.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25971079/)

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

## Getting Started

1. Create a document using the [note.document.add](../document/note-document-add.md) method if you do not already have a page to which the file should be linked
2. Upload a file using the [note.file.add](./note-file-add.md) method and retain the returned `id` or immediately `assetMarkdown`
3. Add `assetMarkdown` to the document text and pass the result in `markdown` to [note.document.update](../document/note-document-update.md). To retrieve a Markdown block for a previously uploaded file, call [note.file.get](./note-file-get.md)
4. Determine the field composition and types using the [note.file.field.list](./note-file-field-list.md) and [note.file.field.get](./note-file-field-get.md) methods if you are building a form, table, or validation on your side


## Limitations and Features

- The size limit applies to the file after Base64 decoding. A positive `main.max_file_size` setting is multiplied by 1024; otherwise, the limit is 25 MiB (26,214,400 bytes). If the limit is exceeded, `note.file.add` returns `NOTE_FILE_TOO_LARGE`
- Allowed extensions are listed under the [fileName parameter of note.file.add](./note-file-add.md#parameters). The check is case insensitive. If the extension is missing or disallowed, the method returns `NOTE_FILE_TYPE_NOT_ALLOWED`
- Uploading requires permission to edit the document; retrieving a file requires permission to view the document. Permissions may come from the knowledge base or be assigned to the document


## Connection with Other Objects

**Documents.** A file is linked to a specific [document](../document/index.md). Calling [note.file.get](./note-file-get.md) requires both identifiers: the file's `id` and the document's `documentId`.

**Knowledge Bases.** A document belongs to a [knowledge base](../collection/index.md). The overall workflow with knowledge bases and documents is described in the [Knowledge Base 2.0 overview](../index.md).


## Overview of Methods {#all-methods}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [note.file.add](./note-file-add.md) | Uploads a file to a document ||
|| [note.file.get](./note-file-get.md) | Returns the document file data and a Markdown block for insertion into the document ||
|| [note.file.field.get](./note-file-field-get.md) | Returns the description of a document file field ||
|| [note.file.field.list](./note-file-field-list.md) | Returns a list of document file fields ||
|#
