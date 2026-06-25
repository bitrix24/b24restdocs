# Documents in Knowledge Base 2.0: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Documents in Knowledge Base 2.0 store the knowledge base content: headings, nested pages, and Markdown text. `note.document.*` methods help create a document, update its heading or content, retrieve the schema for document, tree, and search fields, archive a document branch, or move it to the shopping cart.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25971079/)

## Getting Started

1. Create a knowledge base using the [note.collection.add](../collection/note-collection-add.md) method if it does not already exist
2. Create a root document using the [note.document.add](./note-document-add.md) method
3. Retrieve the document tree using the [note.document.tree.list](./note-document-tree-list.md) method if you need to view the page structure within the knowledge base
4. If a nested structure is required, create child documents using the same `note.document.add` method, passing the `parentId` of the parent document
5. Read an individual document using the [note.document.get](./note-document-get.md) method if you need to retrieve its Markdown and service fields
6. Clarify the composition and types of fields using the `note.document.field.*`, `note.document.tree.field.*`, and `note.document.search.field.*` methods if you are building a form, table, or search on your side
7. Search for documents using the [note.document.search.list](./note-document-search-list.md) method if a topic or part of the text is known
8. Update the heading and content using the [note.document.update](./note-document-update.md) method
9. Archive a document using the [note.document.archive](./note-document-archive.md) method if you need to hide it from active work while retaining the structure
10. Delete a document using the [note.document.delete](./note-document-delete.md) method if you need to move it and its child pages to the shopping cart

## Limitations and Features

- When creating a nested page, the parent document must belong to the same knowledge base. You cannot create a child page in one knowledge base and link it to a document from another
- The size of the `markdown` content is limited to `1 048 576` bytes. If a document becomes too large, the text must be shortened or split into multiple pages
- If a document has unsaved changes from the collaborative editor, an update to `markdown` may result in a conflict. In such cases, you must repeat the request with `overwrite=true` if the content truly needs to be overwritten
- Archiving a document also affects all its child pages. This is useful for archiving an entire section rather than a single page
- Deleting a document also works cascadingly and moves the entire subtree of pages to the shopping cart. Before deleting, consider whether the page has nested materials

{% note tip "User documentation" %}

- [Use the Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25973127/)

{% endnote %}

## Connection with Other Objects

**Knowledge Bases.** A document does not exist independently of a knowledge base. First, a knowledge base is created, and then pages are added to it. Therefore, working with documents usually begins after creating a knowledge base in the [Knowledge Bases](../collection/index.md) section.**Files.** Files are not added to the Knowledge base as a whole, but to a specific page. First, a file is uploaded using the [note.file.add](../file/note-file-add.md) method, then the `assetMarkdown` is retrieved from the method response or obtained using the [note.file.get](../file/note-file-get.md) method. After that, the page content is updated using the [note.document.update](./note-document-update.md) method so that the attachment appears within the document text.

## Overview of Methods {#all-methods}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [note.document.add](./note-document-add.md) | Creates a document ||
|| [note.document.update](./note-document-update.md) | Updates the document title and content ||
|| [note.document.get](./note-document-get.md) | Returns the document with content in Markdown ||
|| [note.document.delete](./note-document-delete.md) | Moves the document and its child pages to the trash ||
|| [note.document.archive](./note-document-archive.md) | Archives the document and its child pages ||
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