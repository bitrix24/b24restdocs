# Knowledge Base 2.0: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Knowledge Base 2.0 helps collect documents by topic, build a general structure, and grant access to materials to the necessary employees. `note.collection.*` methods manage the knowledge base itself as a top-level container: they create a new base, retrieve one or more bases, return a field schema, rename, change the order in the list, archive, move to the Recycle Bin, and restore from the archive and the Recycle Bin.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25971079/)

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

## Getting Started

1. Create a knowledge base using the [note.collection.add](./note-collection-add.md) method
2. Retrieve a list of available knowledge bases using the [note.collection.list](./note-collection-list.md) method if you need to select a specific knowledge base by ID
3. Retrieve data for a single knowledge base using the [note.collection.get](./note-collection-get.md) method if you need to open its card by ID
4. Determine the field composition and types using the [note.collection.field.list](./note-collection-field-list.md) and [note.collection.field.get](./note-collection-field-get.md) methods if you are building a form, table, or validation on your side
5. Use [note.collection.update](./note-collection-update.md) if you need to change the name of a knowledge base, and [note.collection.move](./note-collection-move.md) if you need to change its position in the sidebar
6. Archive a knowledge base using the [note.collection.archive](./note-collection-archive.md) method if you need to hide it from active use while retaining its content. To restore a knowledge base from the archive, use the [note.collection.unarchive](./note-collection-unarchive.md) method
7. Delete a knowledge base using the [note.collection.delete](./note-collection-delete.md) method if you need to move it and its internal documents to the Recycle Bin. To restore a knowledge base from the Recycle Bin, use the [note.collection.restore](./note-collection-restore.md) method


## Limitations and Features

- `note.collection.*` methods work only with the knowledge base as a whole. To create, modify, or delete individual pages, use methods from the [Documents](../document/index.md) section
- Archiving a knowledge base affects not only the container itself but also all documents within it. After archiving, documents remain in the knowledge base but are removed from active use. The reverse method `note.collection.unarchive` restores only the knowledge base itself from the archive; documents are restored separately with the [note.document.unarchive](../document/note-document-unarchive.md) method
- Deleting a knowledge base moves both the knowledge base and all documents within it to the Recycle Bin. The `note.collection.restore` method restores the knowledge base together with these documents. Documents deleted before the knowledge base remain in the Recycle Bin
- The knowledge base state is returned in the `state` field: `live` — active, `archived` — archived, `trashed` — in the Recycle Bin. By default, the `note.collection.list` method returns only active knowledge bases; others are returned by the `state` filter. Knowledge bases in the Recycle Bin are visible only to users with the "Administration" permission for them
- The order of knowledge bases is shared across the entire Bitrix24. All employees see a position change made with `note.collection.move`, so it requires the "Create knowledge bases" permission
- Access depends on the current user's permissions. `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` indicates that access was denied. When retrieving a knowledge base that does not exist or is inaccessible, `note.collection.get` returns `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`
- `note.collection.list` returns up to 200 knowledge bases per request, or up to 50 by default. See the [method page](./note-collection-list.md) for cursor handling and invalid `pagination` values

{% note tip "User documentation" %}

- [Use the Knowledge Base 2.0](https://helpdesk.bitrix24.com/open/25973127/)

{% endnote %}


## Connection with Other Objects

**Documents.** A knowledge base is a container for documents. First, the knowledge base itself is created, and then it is populated with pages and nested sections via methods from the [Documents](../document/index.md) section. All documents belonging to a single topic or department can be kept within one knowledge base.


## Overview of Methods {#all-methods}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [note.collection.add](./note-collection-add.md) | Creates a knowledge base ||
|| [note.collection.update](./note-collection-update.md) | Renames a knowledge base ||
|| [note.collection.get](./note-collection-get.md) | Returns a single knowledge base by identifier ||
|| [note.collection.list](./note-collection-list.md) | Returns a list of knowledge bases available to the user ||
|| [note.collection.move](./note-collection-move.md) | Changes the position of a knowledge base in the shared list ||
|| [note.collection.delete](./note-collection-delete.md) | Moves a knowledge base to the Recycle Bin ||
|| [note.collection.restore](./note-collection-restore.md) | Restores a knowledge base from the Recycle Bin ||
|| [note.collection.archive](./note-collection-archive.md) | Archives a knowledge base ||
|| [note.collection.unarchive](./note-collection-unarchive.md) | Restores a knowledge base from the archive ||
|| [note.collection.field.get](./note-collection-field-get.md) | Returns the description of a knowledge base field ||
|| [note.collection.field.list](./note-collection-field-list.md) | Returns a list of knowledge base fields ||
|#


## Continue Learning

- [{#T}](../document/index.md)
- [{#T}](../file/index.md)
- [{#T}](../index.md)
