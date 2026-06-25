# Knowledge Base 2.0: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Knowledge Base 2.0 helps collect documents by topic, build a general structure, and grant access to materials to the necessary employees. `note.collection.*` methods manage the knowledge base itself as a top-level container: they create a new base, retrieve one or more bases, return a field schema, rename, archive, and move to the shopping cart.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25971079/)

## Getting Started

1. Create a knowledge base using the [note.collection.add](./note-collection-add.md) method
2. Retrieve a list of available knowledge bases using the [note.collection.list](./note-collection-list.md) method if you need to select a specific knowledge base by ID
3. Retrieve data for a single knowledge base using the [note.collection.get](./note-collection-get.md) method if you need to open its card by ID
4. Determine the field composition and types using the [note.collection.field.list](./note-collection-field-list.md) and [note.collection.field.get](./note-collection-field-get.md) methods if you are building a form, table, or validation on your side
5. Use [note.collection.update](./note-collection-update.md) if you need to change the name of a knowledge base
6. Archive a knowledge base using the [note.collection.archive](./note-collection-archive.md) method if you need to hide it from active use while retaining its content
7. Delete a knowledge base using the [note.collection.delete](./note-collection-delete.md) method if you need to move it and its internal documents to the shopping cart

## Limitations and Features

- `note.collection.*` methods work only with the knowledge base as a whole. To create, modify, or delete individual pages, use methods from the [Documents](../document/index.md) section
- Archiving a knowledge base affects not only the container itself but also all documents within it. After archiving, documents remain in the knowledge base but are removed from active use
- Deleting a knowledge base moves both the knowledge base and all documents within it to the shopping cart. If a knowledge base contains many materials, deletion will affect the entire structure at once
- Access to creation, renaming, archiving, and deletion depends on the current user's permissions. If permissions are insufficient, the method will return an access error

{% note tip "User documentation" %}

- [Use the Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25973127/)

{% endnote %}

## Connection with Other Objects

**Documents.** A knowledge base is a container for documents. First, the knowledge base itself is created, and then it is populated with pages and nested sections via methods from the [Documents](../document/index.md) section. All documents belonging to a single topic or department can be kept within one knowledge base.

## Overview of Methods {#all-methods}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [note.collection.add](./note-collection-add.md) | Creates a knowledge base ||
|| [note.collection.update](./note-collection-update.md) | Renames a knowledge base ||
|| [note.collection.get](./note-collection-get.md) | Returns a single knowledge base by identifier ||
|| [note.collection.list](./note-collection-list.md) | Returns a list of knowledge bases available to the user ||
|| [note.collection.delete](./note-collection-delete.md) | Moves a knowledge base to the trash ||
|| [note.collection.archive](./note-collection-archive.md) | Archives a knowledge base ||
|| [note.collection.field.get](./note-collection-field-get.md) | Returns the description of a knowledge base field ||
|| [note.collection.field.list](./note-collection-field-list.md) | Returns a list of knowledge base fields ||
|#

## Continue Learning

- [{#T}](../document/index.md)
- [{#T}](../file/index.md)
- [{#T}](../index.md)