# CRM Timeline Comments: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Timeline comments capture agreements, clarifications, and service notes within the CRM card. For instance, a manager might leave a comment about the outcome of a call or attach a file with a contract.

The methods in this section help manage such records: creating, retrieving, updating, and deleting comments. Events allow for real-time tracking of changes.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Connection to Other Objects

Comments exist only in relation to CRM entities.

**CRM object.** The connection between a comment and an entity is defined by the parameters `ENTITY_TYPE` and `ENTITY_ID`.

- `ENTITY_TYPE` stores the symbolic code of the CRM object type in lowercase, for example `deal` or `dynamic_1046`. You can find the codes of the system types in the [CRM Object Type Reference](../../data-types.md#object_type), and the codes of smart processes in the [Smart Processes](../../universal/user-defined-object-types/index.md) section.
- `ENTITY_ID` stores the identifier of the CRM object. It is returned by list methods and creation methods, such as [crm.item.list](../../universal/crm-item-list.md) and [crm.item.add](../../universal/crm-item-add.md).

The parameters `ENTITY_TYPE` and `ENTITY_ID` are used in the methods [crm.timeline.comment.add](./crm-timeline-comment-add.md) and [crm.timeline.comment.list](./crm-timeline-comment-list.md).

**Files.** A comment can contain attachments in the `FILES` field. The format for file transmission is described in the articles [How to Upload Files](../../../files/how-to-upload-files.md) and [How to Update Files](../../../files/how-to-update-files.md).

The `FILES` field is processed by the methods [crm.timeline.comment.add](./crm-timeline-comment-add.md) and [crm.timeline.comment.update](./crm-timeline-comment-update.md), while attachments are returned in the responses of [crm.timeline.comment.get](./crm-timeline-comment-get.md) and [crm.timeline.comment.list](./crm-timeline-comment-list.md).

## How to Work with Comments

1. Identify the CRM object to which the comment pertains.
2. Check the access permissions for the CRM object. Reading comments requires read permission for the object, and adding them requires permission to edit it. Only the comment author with permission to edit the CRM object can update a comment. A comment can be deleted, with the same permission, by its author or by an administrator of the corresponding CRM object type, taking the pipeline into account.
3. Retrieve the field descriptions using the method [crm.timeline.comment.fields](./crm-timeline-comment-fields.md).
4. To track changes, subscribe to the [comment events](./events/index.md).
5. Add a comment using the method [crm.timeline.comment.add](./crm-timeline-comment-add.md).
6. Retrieve the list of comments using the method [crm.timeline.comment.list](./crm-timeline-comment-list.md) or the data for a specific comment using the method [crm.timeline.comment.get](./crm-timeline-comment-get.md).
7. If needed, update the comment using the method [crm.timeline.comment.update](./crm-timeline-comment-update.md).
8. If needed, delete the comment using the method [crm.timeline.comment.delete](./crm-timeline-comment-delete.md).

If a comment is bound to multiple CRM objects, updating and deleting work with one selected binding: pass `ownerTypeId` and `ownerId` of the required object. Without them, the delete method returns the `MULTIPLE_BINDINGS` error. The specifics are described on the pages of the [crm.timeline.comment.update](./crm-timeline-comment-update.md) and [crm.timeline.comment.delete](./crm-timeline-comment-delete.md) methods, along with errors and access conditions.

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method and the user's permissions for the CRM object

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.timeline.comment.add](./crm-timeline-comment-add.md) | Adds a new comment to the timeline ||
    || [crm.timeline.comment.update](./crm-timeline-comment-update.md) | Updates a comment ||
    || [crm.timeline.comment.get](./crm-timeline-comment-get.md) | Retrieves information about a comment ||
    || [crm.timeline.comment.list](./crm-timeline-comment-list.md) | Retrieves a list of comments for a CRM object ||
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