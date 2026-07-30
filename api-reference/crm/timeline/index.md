# Timeline and Activities in CRM: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The timeline is the primary workspace in the CRM entity detail form. It records:

* system information about working with the entity: stage changes, payments, and the creation of entities based on the current one
* user information: CRM activities (tasks, e-mails, calls) and timeline entries (comments, documents generated from templates, application log entries)

> Quick navigation: [All Methods and Events](#all-methods)
> 
> User documentation: [Timeline in Bitrix24](https://helpdesk.bitrix24.com/open/16767378/), [Universal Activity in Bitrix24](https://helpdesk.bitrix24.com/open/21458972/)

## Activities

Activities in CRM are divided into incoming and scheduled:

* Incoming — activities received from the customer, such as an e-mail or a call. For these activities, it is important to correctly specify the parameter `DIRECTION` = `1` so that the incoming CRM activities counter works
* Scheduled — activities created by employees, such as tasks or universal activities
  
More details about activities and methods for managing them can be found in the article [Activities in CRM: Overview of Methods](./activities/index.md).

## Timeline

Timeline entries are divided into two types:

* Comments. You can add, delete, modify, and retrieve comments through the group of methods [crm.timeline.comment.*](./comments/index.md)
* Log entries. You can add, delete, modify, and retrieve log entries through the group of methods [crm.timeline.logmessage.*](./logmessage/index.md)
  
You can manage the relationships of timeline entries with CRM entities using the methods from the group [crm.timeline.bindings.*](./bindings/index.md)

## Widgets

You can embed an application into activities and timeline entries. Thanks to the embedding, you can use the application without leaving the CRM entity detail form. For embedding, there are special places in the timeline:

* [Button Above the Timeline of the Entity Detail Form](../../widgets/crm/detail-activity.md) `CRM_XXX_DETAIL_ACTIVITY`, `CRM_DYNAMIC_XXX_DETAIL_ACTIVITY`
* [Context Menu Item of the Activity in the Entity Detail Form](../../widgets/crm/activity-timeline-menu.md) `CRM_XXX_ACTIVITY_TIMELINE_MENU`

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Create Activities from Applications](./activities/app-embedding/activity-app.md)

{% endnote %}

## Additional Features

**Text notes** can be added to activities and timeline comments and deleted. Use the group of methods [crm.timeline.note.*](./note/index.md).

**Content blocks** can be added to timeline comments and deleted. Use the group of methods [crm.timeline.layout.blocks.*](./layout-blocks/index.md).

* [Available Content Blocks](./activities/configurable/structure/content-block.md)

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

### Timeline Comments

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.timeline.comment.add](./comments/crm-timeline-comment-add.md)   | Adds a new comment to the timeline ||
    || [crm.timeline.comment.update](./comments/crm-timeline-comment-update.md)  |  Updates a comment ||
    || [crm.timeline.comment.get](./comments/crm-timeline-comment-get.md)   |  Gets information about a comment ||
    || [crm.timeline.comment.list](./comments/crm-timeline-comment-list.md) |  Gets a list of all comments for a CRM item ||
    || [crm.timeline.comment.delete](./comments/crm-timeline-comment-delete.md)  |  Deletes a comment ||
    || [crm.timeline.comment.fields](./comments/crm-timeline-comment-fields.md)  | Gets a list of timeline comment fields ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmTimelineCommentAdd](./comments/events/on-Crm-Timeline-Comment-Add.md) | When creating a new comment in the timeline ||
    || [onCrmTimelineCommentUpdate](./comments/events/on-Crm-Timeline-Comment-Update.md) | When updating a comment in the timeline  ||
    || [onCrmTimelineCommentDelete](./comments/events/on-Crm-Timeline-Comment-Delete.md) | When deleting a comment in the timeline  ||
    |#

{% endlist %}

### Notes for Timeline Entries

#|
|| **Method** | **Description** ||
|| [crm.timeline.note.get](./note/crm-timeline-note-get.md) | Retrieves information about a note ||
|| [crm.timeline.note.save](./note/crm-timeline-note-save.md) | Saves a note ||
|| [crm.timeline.note.delete](./note/crm-timeline-note-delete.md) | Deletes a note ||
|#

### Managing Timeline Entry Relationships

#|
|| **Method** | **Description** ||
|| [crm.timeline.bindings.bind](./bindings/crm-timeline-bindings-bind.md) | Adds a binding of the timeline record with a CRM entity ||
|| [crm.timeline.bindings.list](./bindings/crm-timeline-bindings-list.md) | Retrieves a list of relationships for a timeline entry ||
|| [crm.timeline.bindings.unbind](./bindings/crm-timeline-bindings-unbind.md) | Removes the binding of the timeline record with a CRM entity ||
|| [crm.timeline.bindings.fields](./bindings/crm-timeline-bindings-fields.md) | Retrieves the fields of the relationship between CRM entities and timeline entries ||
|#

### Additional Content Blocks

#|
|| **Method** | **Description** ||
|| [crm.timeline.layout.blocks.set](./layout-blocks/crm-timeline-layout-blocks-set.md) | Sets a set of additional content blocks in the timeline entry ||
|| [crm.timeline.layout.blocks.get](./layout-blocks/crm-timeline-layout-blocks-get.md) | Retrieves the set of additional content blocks installed by the application for the timeline entry ||
|| [crm.timeline.layout.blocks.delete](./layout-blocks/crm-timeline-layout-blocks-delete.md) | Deletes the set of additional content blocks installed by the application for the timeline entry ||
|#

### Application Log Entry Journal

#|
|| **Method** | **Description** ||
|| [crm.timeline.logmessage.add](./logmessage/crm-timeline-logmessage-add.md) | Adds a new log entry to the timeline ||
|| [crm.timeline.logmessage.get](./logmessage/crm-timeline-logmessage-get.md) | Retrieves information about a log entry ||
|| [crm.timeline.logmessage.list](./logmessage/crm-timeline-logmessage-list.md) | Retrieves a list of all log entries for a specific entity ||
|| [crm.timeline.logmessage.delete](./logmessage/crm-timeline-logmessage-delete.md) | Deletes a log entry ||
|| [crm.timeline.icon.*](./logmessage/icons/index.md) | Manages entry icons ||
|| [crm.timeline.logo.*](./logmessage/logo/index.md) | Manages entry logos ||
|#

### Actions with Timeline Entries

#|
|| **Method** | **Description** ||
|| [crm.timeline.item.pin](./actions/crm-timeline-item-pin.md) | Pins a record in the timeline ||
|| [crm.timeline.item.unpin](./actions/crm-timeline-item-unpin.md) | Unpins a record in the timeline ||
|#
