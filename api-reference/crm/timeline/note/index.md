# Notes on Timeline Records: Overview of Methods

The group of methods `crm.timeline.note.*` works with text notes associated with timeline records. A note can be saved, retrieved, or deleted.

For example, if you need to save a call transcript to its corresponding deal timeline record, it can be added as a note and retrieved or deleted as necessary.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Getting Started

1. Identify the CRM entity: `ownerTypeId` and `ownerId`
2. Define the record type `itemType`
3. Retrieve the `itemId` of the record based on `itemType`
4. Save the note using the [crm.timeline.note.save](./crm-timeline-note-save.md) method
5. To read the note, use the [crm.timeline.note.get](./crm-timeline-note-get.md) method
6. Delete the unnecessary note using the [crm.timeline.note.delete](./crm-timeline-note-delete.md) method

## Relationship with Other Objects

**CRM Entities.** The note is linked to a specific CRM entity: the entity type is passed in `ownerTypeId`, and the identifier is passed in `ownerId`. Typical values for `ownerTypeId` are listed in the article [CRM Object Type](../../data-types.md#object_type). You can obtain the `ownerId` using the universal method [crm.item.list](../../universal/crm-item-list.md).

**Timeline History Records.** The note is added to a comment in the timeline of the CRM entity. To link it, pass `itemType = 1` and the record identifier `itemId`. To obtain `itemId`, use the [crm.timeline.comment.add](../comments/crm-timeline-comment-add.md) method when creating a comment or the [crm.timeline.comment.list](../comments/crm-timeline-comment-list.md) method for existing records.

**CRM Activities.** A note can also be added to a CRM activity in the timeline—such as a call, email, meeting, or task. To link it, pass `itemType = 2` and the activity identifier `itemId`. Retrieve `itemId` from the response of the [crm.activity.add](../activities/activity-base/crm-activity-add.md) method or from the list using the [crm.activity.list](../activities/activity-base/crm-activity-list.md) method.

**Universal CRM Activities.** A universal activity is an advanced type of activity that includes synchronization with a calendar, selection of a meeting place, and negotiation room. To link it, just like a regular activity, pass `itemType = 2` and the identifier `itemId`. Get `itemId` from the response of the [crm.activity.todo.add](../activities/todo/crm-activity-todo-add.md) method or from the list using the [crm.activity.list](../activities/activity-base/crm-activity-list.md) method.

{% note tip "Typical use-cases and scenarios" %}

- [How to add a comment to the timeline of a SPA](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-comment-to-spa.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [crm.timeline.note.save](./crm-timeline-note-save.md) | Saves a note ||
|| [crm.timeline.note.get](./crm-timeline-note-get.md) | Returns information about the note ||
|| [crm.timeline.note.delete](./crm-timeline-note-delete.md) | Deletes a note ||
|#