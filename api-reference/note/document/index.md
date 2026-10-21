# Documents in Knowledge Base 2.0: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Documents in Knowledge Base 2.0 store the knowledge base content: headings, nested pages, and Markdown text. `note.document.*` methods help create a document, find it in the tree, the list, or the search, update its heading or content, move a document branch, retrieve the schema for document, tree, and search fields, archive a branch or move it to the Recycle Bin, and restore it.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25971079/)

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

## Getting Started

1. Create a knowledge base using the [note.collection.add](../collection/note-collection-add.md) method if it does not already exist
2. Create a root document using the [note.document.add](./note-document-add.md) method
3. If a nested structure is required, create child documents using the same `note.document.add` method, passing the `parentId` of the parent document
4. Retrieve the page structure of a single knowledge base using the [note.document.tree.list](./note-document-tree-list.md) method. To go through all documents available to the user in all knowledge bases, use [note.document.list](./note-document-list.md)
5. Read an individual document using the [note.document.get](./note-document-get.md) method if you need to retrieve its Markdown and service fields
6. Search for documents using the [note.document.search.list](./note-document-search-list.md) method if a topic or part of the text is known
7. Update the heading and content using the [note.document.update](./note-document-update.md) method. To move a document to another branch or knowledge base, or to change its position among its siblings, use [note.document.move](./note-document-move.md)
8. Clarify the composition and types of fields using the `note.document.field.*`, `note.document.tree.field.*`, and `note.document.search.field.*` methods if you are building a form, table, or search on your side
9. Archive a document using the [note.document.archive](./note-document-archive.md) method if you need to hide it from active work while retaining the structure. To restore a document from the archive, use the [note.document.unarchive](./note-document-unarchive.md) method
10. Delete a document using the [note.document.delete](./note-document-delete.md) method if you need to move it and its child pages to the Recycle Bin. To restore a document from the Recycle Bin, use the [note.document.restore](./note-document-restore.md) method

## Limitations and Features

- When creating a nested page, the parent document must belong to the same knowledge base. You cannot create a child page in one knowledge base and link it to a document from another
- The size of the `markdown` content is limited to `1 048 576` bytes. If a document becomes too large, the text must be shortened or split into multiple pages
- You can insert the `assetMarkdown` block returned by [note.file.add](../file/note-file-add.md) into `markdown`, for example, `[[image fileId=5001]]`. For images and videos, `width` and `align` are available; their format is described in [note.document.update](./note-document-update.md). These parameters do not apply to regular files
- If a document has unsaved changes from the collaborative editor, an update to `markdown` may result in a conflict. In such cases, you must repeat the request with `overwrite=true` if the content truly needs to be overwritten
- Each document is in one of the states returned in the `state` field: `live` — active, `archived` — archived, `trashed` — in the Recycle Bin. The `note.document.tree.list` method returns only active documents; `note.document.list` returns active documents by default and others by the `state` filter. The `note.document.get` method reads a document in any state
- Archiving a document also affects all its child pages. The reverse method `note.document.unarchive` works differently: it restores only the specified document. To restore an entire branch, call it for each document from top to bottom; otherwise, the child pages end up at the top level of the knowledge base
- Deleting a document also works cascadingly and moves the entire subtree of pages to the Recycle Bin. The `note.document.restore` method restores this subtree in full. If the entire knowledge base is in the Recycle Bin, the document cannot be restored on its own — restore the knowledge base with the [note.collection.restore](../collection/note-collection-restore.md) method
- Moving with `note.document.move` transfers the document together with all its child pages. To move a document to another knowledge base, you need the "Edit" permission in both knowledge bases
- The `note.document.list` method does not report access denials: inaccessible documents are simply excluded from the response, so an empty list is a normal result

{% note tip "User documentation" %}

- [Use the Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25973127/)

{% endnote %}


## Connection with Other Objects

**Knowledge Bases.** When created, a document is linked to a [knowledge base](../collection/index.md) through `collectionId`. You can change the document's knowledge base with the [note.document.move](./note-document-move.md) method.

**Files.** Attachments are linked to a specific document. The upload and insertion steps are described in the [file methods overview](../file/index.md).


## Overview of Methods {#all-methods}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [note.document.add](./note-document-add.md) | Creates a document ||
|| [note.document.update](./note-document-update.md) | Updates the document title and content ||
|| [note.document.get](./note-document-get.md) | Returns the document with content in Markdown ||
|| [note.document.delete](./note-document-delete.md) | Moves the document and its child pages to the Recycle Bin ||
|| [note.document.restore](./note-document-restore.md) | Restores the document and its child pages from the Recycle Bin ||
|| [note.document.archive](./note-document-archive.md) | Archives the document and its child pages ||
|| [note.document.unarchive](./note-document-unarchive.md) | Restores the document from the archive ||
|| [note.document.move](./note-document-move.md) | Moves the document to another knowledge base, under another parent document, or to another position ||
|| [note.document.list](./note-document-list.md) | Returns all documents available to the user, page by page ||
|| [note.document.tree.list](./note-document-tree-list.md) | Returns the document tree of a single knowledge base ||
|| [note.document.search.list](./note-document-search-list.md) | Searches for documents by title and content ||
|| [note.document.field.get](./note-document-field-get.md) | Returns the description of a document field ||
|| [note.document.field.list](./note-document-field-list.md) | Returns a list of document fields ||
|| [note.document.tree.field.get](./note-document-tree-field-get.md) | Returns the description of a document tree field ||
|| [note.document.tree.field.list](./note-document-tree-field-list.md) | Returns a list of document tree fields ||
|| [note.document.search.field.get](./note-document-search-field-get.md) | Returns the description of a document search result field ||
|| [note.document.search.field.list](./note-document-search-field-list.md) | Returns a list of document search result fields ||
|#


## Continue Learning

- [{#T}](../collection/index.md)
- [{#T}](../file/index.md)
- [{#T}](../index.md)
