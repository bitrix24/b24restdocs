# Timeline Comments in REST 3.0: Method Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These methods are part of REST 3.0. The calling conventions and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

Timeline comments are text notes in a CRM item form: agreements with clients, call outcomes, and internal notes. The methods in this group add a comment to an item’s timeline, retrieve a comment by ID, and retrieve a list of comments for one item.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Timeline in CRM Item Form](https://helpdesk.bitrix24.com/open/25816403/)

{% note warning "" %}

REST 3.0 methods cannot edit or delete comments, or attach files to them. Use the previous version’s methods: [crm.timeline.comment.update](../../timeline/comments/crm-timeline-comment-update.md), [crm.timeline.comment.delete](../../timeline/comments/crm-timeline-comment-delete.md), and [crm.timeline.comment.add](../../timeline/comments/crm-timeline-comment-add.md) with the `FILES` field.

{% endnote %}

## Object Types and Method Names

The `<type>` segment in the method name specifies the object type.

#|
|| **Object Type** | **Method Name Segment** ||
|| Deal | `deal` ||
|| Lead | `lead` ||
|| Contact | `contact` ||
|| Company | `company` ||
|| My company | `mycompany` ||
|| Estimate | `quote` ||
|| Invoice | `smartinvoice` ||
|| CRM document | `smartdocument` ||
|| B2E document | `smartb2edocument` ||
|| SPA | `smartprocess<entityTypeId>`, for example, `smartprocess1038` ||
|#

The [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method returns SPA type IDs `entityTypeId`. Method names must be lowercase: `crm.smartinvoice.timeline.comment.add`.

Company and my company are different types. The `crm.company.timeline.comment.*` methods cannot find my company, and `crm.mycompany.timeline.comment.*` cannot find a regular company.

## Getting Started

1. Retrieve the item ID, for example, using [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md)
2. Add a comment using [crm.&lt;type&gt;.timeline.comment.add](./crm-entity-timeline-comment-add-rest-v3.md): pass the item’s `ownerId` and `text`. The method returns the comment `id`
3. Retrieve the item’s comments using [crm.&lt;type&gt;.timeline.comment.list](./crm-entity-timeline-comment-list-rest-v3.md) with the filter `[["ownerId","=",<item ID>]]`, or a single comment using [crm.&lt;type&gt;.timeline.comment.get](./crm-entity-timeline-comment-get-rest-v3.md). Pass `select`, for example, `["id","text","authorId","createdTime"]`: without it, the methods return only `id`

## Specifics

- **Without `select`, only the ID is returned.** Without `select`, `get` and `list` return only the `id` field for each comment
- **Comments for one item.** The `list` filter accepts exactly one condition `["ownerId","=",<ID>]`. It accepts no other fields or operators, and sorting is supported only by `createdTime`
- **Unprocessed text.** BBCode and HTML tags are retained and returned as is. Empty text and attachment tags `[DISK FILE ID=…]` cause an error. Text longer than 65,535 bytes is truncated without an error
- **The calling user is the author.** The `authorId` field cannot be set
- **Permissions.** Adding a comment requires permission to edit the item, and reading requires permission to view it. For my company, permission to edit or view my companies is required. If permission is missing, the methods return a “not found” error rather than “access denied”

## Differences from the Previous Version

The previous version’s methods are described in [CRM Timeline Comments](../../timeline/comments/index.md).

#|
|| **Previous Version** | **REST 3.0** ||
|| Item type in `ENTITY_TYPE`, for example, `deal`, and item in `ENTITY_ID` | Item type in the method name: `crm.deal.timeline.comment.add`, and item in `ownerId` ||
|| Uppercase fields: `ID`, `COMMENT`, `AUTHOR_ID`, `CREATED` | camelCase fields: `id`, `text`, `authorId`, `createdTime` ||
|| [crm.timeline.comment.update](../../timeline/comments/crm-timeline-comment-update.md) and [crm.timeline.comment.delete](../../timeline/comments/crm-timeline-comment-delete.md) | No equivalents; use the previous version’s methods ||
|| Files in `FILES` in [crm.timeline.comment.add](../../timeline/comments/crm-timeline-comment-add.md) and in [get](../../timeline/comments/crm-timeline-comment-get.md) and [list](../../timeline/comments/crm-timeline-comment-list.md) responses | Files are neither passed nor returned. Attachment tags in the text cause an error when adding a comment and are removed from the text when reading ||
|| Emoji in the text are returned in an internal format, for example, `:f09f9880:` | Emoji are returned as is ||
|| [crm.timeline.comment.list](../../timeline/comments/crm-timeline-comment-list.md): 50 records per page, `start` parameter | `pagination` with a `limit` of up to 1000, `page`, and `offset` ||
|| Field descriptions: [crm.timeline.comment.fields](../../timeline/comments/crm-timeline-comment-fields.md) | Field descriptions: `crm.<type>.timeline.comment.field.list` and `crm.<type>.timeline.comment.field.get` ||
|#

A comment created using a REST 3.0 method can be edited and deleted using the previous version’s methods with the same ID.

## Connection to Other Objects

A comment is linked to a CRM item and an author.

**CRM items.** A comment is linked to an item: the item ID is in `ownerId`, and the object type is in `ownerTypeId`, for example, `2` for a deal. Retrieve the item using [crm.&lt;type&gt;.get](../items/crm-entity-get-rest-v3.md). The [crm.dictionary.entitytype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method returns object type IDs.

**Users.** The comment author is specified in `authorId`. The [user.get](../../../user/user-get.md) method returns user data.

**Events.** Adding a comment using a REST 3.0 method triggers the [onCrmTimelineCommentAdd](../../timeline/comments/events/on-Crm-Timeline-Comment-Add.md) event. Comment events are described in [Comment Events](../../timeline/comments/events/index.md).

## Field Methods

The `crm.<type>.timeline.comment.field.list` and `crm.<type>.timeline.comment.field.get` methods return comment field descriptions. They work like other REST 3.0 field methods: `field.list` accepts `select` with a list of description attributes, and `field.get` also accepts `name` with the field name, for example, `{"name":"text"}`. The `filterable` and `sortable` attributes in the response show that filtering is supported only by `ownerId`, and sorting only by `createdTime`. The fields themselves are described in the table on the [crm.&lt;type&gt;.timeline.comment.get](./crm-entity-timeline-comment-get-rest-v3.md#item) page.

## Method Overview {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: adding requires permission to edit the item; reading requires permission to view the item

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.timeline.comment.add](./crm-entity-timeline-comment-add-rest-v3.md) | Adds a comment to an item’s timeline ||
|| [crm.&lt;type&gt;.timeline.comment.get](./crm-entity-timeline-comment-get-rest-v3.md) | Retrieves a comment by ID ||
|| [crm.&lt;type&gt;.timeline.comment.list](./crm-entity-timeline-comment-list-rest-v3.md) | Retrieves comments for one item ||
|| `crm.<type>.timeline.comment.field.list` | Retrieves comment field descriptions ||
|| `crm.<type>.timeline.comment.field.get` | Retrieves a comment field description by name ||
|#
