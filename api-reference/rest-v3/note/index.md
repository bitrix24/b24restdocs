# Knowledge Base 2.0 in REST 3.0: Section Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Knowledge Base 2.0 helps collect internal company materials: regulations, instructions, training texts, and other documents. You can maintain multiple knowledge bases, build a page hierarchy, restrict access, and collaborate on content.

The section methods work with several groups of objects:

- [Knowledge Bases](./collection/index.md) — create a knowledge base, retrieve data and field schemas, rename, archive, and move to the shopping cart
- [Documents](./document/index.md) — create documents, retrieve the page tree and content, search for documents, retrieve field schemas for documents, the tree, and search, and archive or delete page subtrees
- [Files](./file/index.md) — upload a file to a document, retrieve data and field schemas, and return a Markdown block for inserting an attachment

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25971079/)

## Getting Started

1. Create a knowledge base using the [note.collection.add](./collection/note-collection-add.md) method if you do not already have a container for documents
2. Retrieve a list of available knowledge bases using the [note.collection.list](./collection/note-collection-list.md) method if you need to work with an existing structure
3. Retrieve data for a single knowledge base using the [note.collection.get](./collection/note-collection-get.md) method if you need to open a selected knowledge base by its ID
4. Retrieve knowledge base fields using the [note.collection.field.list](./collection/note-collection-field-list.md) and [note.collection.field.get](./collection/note-collection-field-get.md) methods if you are building a form or a table on your side
5. Create a root document using the [note.document.add](./document/note-document-add.md) method
6. Retrieve the document tree using the [note.document.tree.list](./document/note-document-tree-list.md) method if you need to display the page structure
7. Read an individual document using the [note.document.get](./document/note-document-get.md) method or search for documents using the [note.document.search.list](./document/note-document-search-list.md) method
8. Retrieve fields for documents, the tree, and search using methods `note.document.field.*`, `note.document.tree.field.*`, `note.document.search.field.*`
9. Update a document heading and content using the [note.document.update](./document/note-document-update.md) method
10. Upload images, videos, and standard files using the [note.file.add](./file/note-file-add.md) method
11. Use the `assetMarkdown` from the [note.file.add](./file/note-file-add.md) response or retrieve it using the [note.file.get](./file/note-file-get.md) method, then add the attachment to a document via [note.document.update](./document/note-document-update.md)
12. Retrieve file fields using the [note.file.field.list](./file/note-file-field-list.md) and [note.file.field.get](./file/note-file-field-get.md) methods if you are building your own form or table
13. Archive or delete a knowledge base and documents using the corresponding methods when you need to finish working with the materials

## Limitations and Recommendations

- Document archiving and deletion methods operate on the entire subtree rather than a single page. If a document has child pages, they will also be archived or moved to the shopping cart.
- Access to view and edit Knowledge bases, documents, and files depends on the current user's permissions. The same scenario may be available to some employees and unavailable to others.

{% note tip "User documentation" %}

- [Use the Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25973127/)

{% endnote %}

## Connection with Other Objects

**Documents.** The Knowledge base is populated with documents. You can first create a root page and then add child pages to it to build a tree of materials organized by topic. To do this, specify the Knowledge base where the document should appear when creating a document, and for nested pages, additionally provide the parent document.

**Files.** Files are added to a specific document rather than the entire Knowledge base. First, upload a file using the [note.file.add](./file/note-file-add.md) method, then retrieve the `assetMarkdown` from the method response or obtain it using the [note.file.get](./file/note-file-get.md) method. After that, update the document content using the [note.document.update](./document/note-document-update.md) method so that the attachment appears in the page text.

## Overview of Methods {#all-methods}

> Scope: [`note`](../../scopes/permissions.md)
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
- [{#T}](../index.md)