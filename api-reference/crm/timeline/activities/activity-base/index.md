# CRM Activities in the Timeline: Methods Overview

A CRM activity is a timeline entry that records an interaction with a customer. It is linked to a CRM item and displayed within its card.

The methods in this section work with CRM activities: they create and update universal activities, retrieve an activity by identifier, retrieve a list by filter, delete an activity, retrieve a call transcription, and describe fields.

For example, if you need to remove a call from a deal's timeline, retrieve the list of activities for that deal and delete the required activity.

{% note warning "" %}

**DEPRECATED**

Development of the [crm.activity.add](./crm-activity-add.md) and [crm.activity.update](./crm-activity-update.md) methods has been discontinued.
Use the [crm.activity.todo.add](../todo/crm-activity-todo-add.md) and [crm.activity.todo.update](../todo/crm-activity-todo-update.md) methods instead.

{% endnote %}

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Timeline in the CRM Card](https://helpdesk.bitrix24.com/open/23960160/)

## Getting Started

1. Identify the CRM entity: `ownerTypeId` and `ownerId`
2. Create a universal activity using the [crm.activity.todo.add](../todo/crm-activity-todo-add.md) method
3. Verify the created activity using the [crm.activity.get](./crm-activity-get.md) method
4. To modify a universal activity, use the [crm.activity.todo.update](../todo/crm-activity-todo-update.md) method
5. To find activities by filter, use the [crm.activity.list](./crm-activity-list.md) method
6. Delete the required activity using the [crm.activity.delete](./crm-activity-delete.md) method
7. To retrieve a completed call transcription, use the [crm.activity.call.getTranscript](./crm-activity-call-get-transcript.md) method
8. To check the activity field structure, use the [crm.activity.fields](./crm-activity-fields.md) method
9. Check the communication field structure using the [crm.activity.communication.fields](./crm-activity-communication-fields.md) method

## Connection with Other Objects

**CRM items.** An activity is linked to a CRM item via the `ownerTypeId` and `ownerId` parameters. The [crm.enum.ownertype](../../../auxiliary/enum/crm-enum-owner-type.md) method returns standard values `ownerTypeId` for leads, deals, contacts, and companies. For SPAs, use the [crm.type.list](../../../universal/user-defined-object-types/crm-type-list.md) method. Retrieve the `ownerId` identifier via the universal [crm.item.list](../../../universal/crm-item-list.md) method.

**Activity links.** A single activity can be linked to multiple CRM items. To manage links—adding, moving, and deleting—use the [crm.activity.binding.*](../binding/index.md) method group.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [crm.activity.todo.add](../todo/crm-activity-todo-add.md) | Adds a universal activity to the timeline ||
|| [crm.activity.todo.update](../todo/crm-activity-todo-update.md) | Updates a universal activity ||
|| [crm.activity.get](./crm-activity-get.md) | Returns an activity by its identifier ||
|| [crm.activity.list](./crm-activity-list.md) | Returns a list of activities based on a filter ||
|| [crm.activity.delete](./crm-activity-delete.md) | Deletes an activity ||
|| [crm.activity.call.getTranscript](./crm-activity-call-get-transcript.md) | Returns a ready-made call transcription ||
|| [crm.activity.fields](./crm-activity-fields.md) | Returns the description of the activity fields ||
|| [crm.activity.communication.fields](./crm-activity-communication-fields.md) | Returns the description of the communication fields of the activity ||
|#

### Deprecated Methods

#|
|| **Method** | **Description** ||
|| [crm.activity.add](./crm-activity-add.md) | Creates a system activity ||
|| [crm.activity.update](./crm-activity-update.md) | Updates a system activity ||
|#
