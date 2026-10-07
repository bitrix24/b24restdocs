# Knowledge Base 2.0 in REST 3.0: Section Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Knowledge Base 2.0 helps collect internal company materials: regulations, instructions, training texts, and other documents. You can maintain multiple knowledge bases, build a page hierarchy, restrict access, and collaborate on content.

The section methods work with several groups of objects:

- [Knowledge Bases](./collection/index.md) — create a knowledge base, retrieve data and field schemas, rename, archive, and move to the shopping cart
- [Documents](./document/index.md) — create documents, retrieve the page tree and content, search for documents, retrieve field schemas for documents, the tree, and search, and archive or delete page subtrees
- [Files](./file/index.md) — upload a file to a document, retrieve data and field schemas, and return a Markdown block for inserting an attachment

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25971079/)

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

## Getting Started

1. Create a knowledge base with [note.collection.add](./collection/note-collection-add.md), or choose an existing one with [note.collection.list](./collection/note-collection-list.md)
2. Create a document with [note.document.add](./document/note-document-add.md). For a child page, pass the parent document's `parentId`
3. Retrieve the structure with [note.document.tree.list](./document/note-document-tree-list.md), page content with [note.document.get](./document/note-document-get.md), and text matches with [note.document.search.list](./document/note-document-search-list.md)
4. If needed, upload an attachment with [note.file.add](./file/note-file-add.md), add its `assetMarkdown` to the text, and save the document with [note.document.update](./document/note-document-update.md)
5. To configure an integration, inspect knowledge base, document, and file fields with the `*.field.list` and `*.field.get` methods in the [methods table](#all-methods)


## Limitations and Recommendations

- Archiving or deleting a knowledge base affects all documents in it. Deletion moves data to the trash; restoration is available through the interface
- The `markdown` content must not exceed 1,048,576 bytes when creating or updating a document. Exceeding the limit returns `NOTE_MARKDOWN_TOO_LARGE`. Additional limits are described in the [document](./document/index.md) and [file](./file/index.md) overviews
- Document archiving and deletion methods operate on the entire subtree rather than a single page. If a document has child pages, they will also be archived or moved to the shopping cart.
- Access to view and edit Knowledge bases, documents, and files depends on the current user's permissions. The same scenario may be available to some employees and unavailable to others.

{% note tip "User documentation" %}

- [Use the Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25973127/)

{% endnote %}


## Connection with Other Objects

**Documents.** The `collectionId` field links a document to a [knowledge base](./collection/index.md), and `parentId` links it to a parent page. The tree model and main fields are described in the [document overview](./document/index.md).

**Files.** Attachments are linked to a document through `documentId` and represented by special blocks in its Markdown. Attachment types and upload limits are described in the [file overview](./file/index.md).


## Overview of Methods {#all-methods}

> Scope: [`note`](../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

### Knowledge Bases

#|
|| **Method** | **Description** ||
|| [note.collection.add](./collection/note-collection-add.md) | Creates a knowledge base ||
|| [note.collection.update](./collection/note-collection-update.md) | Renames a knowledge base ||
|| [note.collection.get](./collection/note-collection-get.md) | Returns a single knowledge base by ID ||
|| [note.collection.list](./collection/note-collection-list.md) | Returns a list of knowledge bases available to the user ||
|| [note.collection.delete](./collection/note-collection-delete.md) | Moves a knowledge base to the trash ||
|| [note.collection.archive](./collection/note-collection-archive.md) | Archives a knowledge base ||
|| [note.collection.field.get](./collection/note-collection-field-get.md) | Returns the description of a knowledge base field ||
|| [note.collection.field.list](./collection/note-collection-field-list.md) | Returns a list of knowledge base fields ||
|#

### Documents

#|
|| **Method** | **Description** ||
|| [note.document.add](./document/note-document-add.md) | Creates a document ||
|| [note.document.update](./document/note-document-update.md) | Updates the title and content of a document ||
|| [note.document.get](./document/note-document-get.md) | Returns a document with content in Markdown ||
|| [note.document.delete](./document/note-document-delete.md) | Moves a document and its child pages to the trash ||
|| [note.document.archive](./document/note-document-archive.md) | Archives a document and its child pages ||
|| [note.document.tree.list](./document/note-document-tree-list.md) | Returns the document tree of a single knowledge base ||
|| [note.document.search.list](./document/note-document-search-list.md) | Searches for documents by title and content ||
|| [note.document.field.get](./document/note-document-field-get.md) | Returns the description of a document field ||
|| [note.document.field.list](./document/note-document-field-list.md) | Returns a list of document fields ||
|| [note.document.tree.field.get](./document/note-document-tree-field-get.md) | Returns the description of a document tree field ||
|| [note.document.tree.field.list](./document/note-document-tree-field-list.md) | Returns a list of document tree fields ||
|| [note.document.search.field.get](./document/note-document-search-field-get.md) | Returns the description of a document search result field ||
|| [note.document.search.field.list](./document/note-document-search-field-list.md) | Returns a list of document search result fields ||
|#

### Files

#|
|| **Method** | **Description** ||
|| [note.file.add](./file/note-file-add.md) | Uploads a file to a document ||
|| [note.file.get](./file/note-file-get.md) | Returns the document file data and a Markdown block for insertion into the document ||
|| [note.file.field.get](./file/note-file-field-get.md) | Returns the description of a document file field ||
|| [note.file.field.list](./file/note-file-field-list.md) | Returns a list of document file fields ||
|#


## Continue Learning

- [{#T}](./collection/index.md)
- [{#T}](./document/index.md)
- [{#T}](./file/index.md)
- [{#T}](../rest-v3.md)
