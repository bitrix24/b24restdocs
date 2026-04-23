# CRM Timeline Comments: Overview of Methods and Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Timeline comments capture agreements, clarifications, and service notes within the CRM card. For instance, a manager might leave a comment about the outcome of a call or attach a file with a contract.

The methods in this section help manage such records: creating, retrieving, updating, and deleting comments. Events allow for real-time tracking of changes.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Connection to Other Objects

Comments exist only in relation to CRM entities.

**CRM Entity.** The connection between a comment and an entity is defined by the parameters `ENTITY_TYPE` and `ENTITY_ID`.

- `ENTITY_TYPE` stores the type of CRM object. You can find the type values in the [CRM Object Type Reference](../../data-types.md#object_type).  
- `ENTITY_ID` stores the identifier of the CRM entity. It is returned by list methods and creation methods, such as [crm.item.list](../../universal/crm-item-list.md) and [crm.item.add](../../universal/crm-item-add.md).  

The parameters `ENTITY_TYPE` and `ENTITY_ID` are used in the methods [crm.timeline.comment.add](./crm-timeline-comment-add.md) and [crm.timeline.comment.list](./crm-timeline-comment-list.md).

**Files.** A comment can contain attachments in the `FILES` field. The format for file transmission is described in the articles [How to Upload Files](../../../files/how-to-upload-files.md) and [How to Update Files](../../../files/how-to-update-files.md).

The `FILES` field is processed by the methods [crm.timeline.comment.add](./crm-timeline-comment-add.md) and [crm.timeline.comment.update](./crm-timeline-comment-update.md), while attachments are returned in the responses of [crm.timeline.comment.get](./crm-timeline-comment-get.md) and [crm.timeline.comment.list](./crm-timeline-comment-list.md).  

## How to Work with Comments

1. Identify the CRM entity to which the comment pertains.
2. Check the access permissions for the CRM entity; otherwise, the methods may return an `Access denied` error. Access depends on the current user's rights to the specific entity.
3. Retrieve the list of available fields using the method [crm.timeline.comment.fields](./crm-timeline-comment-fields.md).
4. Add a comment using the method [crm.timeline.comment.add](./crm-timeline-comment-add.md).
5. Retrieve the list of comments using the method [crm.timeline.comment.list](./crm-timeline-comment-list.md) or the data for a specific comment using the method [crm.timeline.comment.get](./crm-timeline-comment-get.md).
6. Modify the comment using the method [crm.timeline.comment.update](./crm-timeline-comment-update.md).
7. Delete any unnecessary comments using the method [crm.timeline.comment.delete](./crm-timeline-comment-delete.md).
8. To track changes, subscribe to events in the [Comment Events](./events/index.md) section.

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: `any user`

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.timeline.comment.add](./crm-timeline-comment-add.md) | Adds a new comment to the timeline ||
    || [crm.timeline.comment.update](./crm-timeline-comment-update.md) | Updates a comment ||
    || [crm.timeline.comment.get](./crm-timeline-comment-get.md) | Retrieves information about a comment ||
    || [crm.timeline.comment.list](./crm-timeline-comment-list.md) | Retrieves a list of comments for a CRM entity ||
    || [crm.timeline.comment.delete](./crm-timeline-comment-delete.md) | Deletes a comment ||
    || [crm.timeline.comment.fields](./crm-timeline-comment-fields.md) | Retrieves a list of timeline comment fields ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmTimelineCommentAdd](./events/on-Crm-Timeline-Comment-Add.md) | When a new comment is created in the timeline manually or via the method [crm.timeline.comment.add](./crm-timeline-comment-add.md) ||
    || [onCrmTimelineCommentUpdate](./events/on-Crm-Timeline-Comment-Update.md) | When a comment is updated in the timeline manually or via the method [crm.timeline.comment.update](./crm-timeline-comment-update.md) ||
    || [onCrmTimelineCommentDelete](./events/on-Crm-Timeline-Comment-Delete.md) | When a comment is deleted in the timeline manually or via the method [crm.timeline.comment.delete](./crm-timeline-comment-delete.md) ||
    |#

{% endlist %}