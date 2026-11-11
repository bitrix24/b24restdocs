# CRM Activities in the Timeline: Methods Overview

A CRM activity is a timeline entry that records an interaction with a customer. It is linked to a CRM item and displayed within its card.

The methods for working with CRM activities create and update universal activities, retrieve an activity by identifier, retrieve a list by filter, delete an activity, retrieve a call transcription, and describe fields.

For example, if you need to remove a call from a deal's timeline, retrieve the list of activities for that deal and delete the required activity.

{% note warning "" %}

**DEPRECATED**

Development of the [crm.activity.add](./crm-activity-add.md) and [crm.activity.update](./crm-activity-update.md) methods has been discontinued.
For universal activities, use the [crm.activity.todo.add](../todo/crm-activity-todo-add.md) and [crm.activity.todo.update](../todo/crm-activity-todo-update.md) methods. For configurable activities, use the [Configurable Activity Methods](../configurable/index.md).

{% endnote %}

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Getting Started

1. Identify the CRM item type `ownerTypeId` and identifier `ownerId`, as well as the activity deadline `deadline`
2. Create a universal activity using the [crm.activity.todo.add](../todo/crm-activity-todo-add.md) method
3. Retrieve the created universal activity using the [crm.activity.get](./crm-activity-get.md) method
4. To modify a universal activity, use the [crm.activity.todo.update](../todo/crm-activity-todo-update.md) method
5. To find activities by filter, use the [crm.activity.list](./crm-activity-list.md) method
6. Delete the required activity using the [crm.activity.delete](./crm-activity-delete.md) method

To retrieve a call transcription or a description of activity and communication fields, choose a method in the [Overview of Methods](#all-methods).

## Connection with Other Objects

**CRM items.** When creating a universal activity, specify the CRM item type and identifier in the `ownerTypeId` and `ownerId` parameters. The [crm.enum.ownertype](../../../auxiliary/enum/crm-enum-owner-type.md) method returns standard values `ownerTypeId` for leads, deals, contacts, and companies. For SPAs, use the [crm.type.list](../../../universal/user-defined-object-types/crm-type-list.md) method. Retrieve the `ownerId` identifier via the universal [crm.item.list](../../../universal/crm-item-list.md) method.

**Activity links.** A single activity can be linked to multiple CRM items. To manage links—adding, moving, and deleting—use the [crm.activity.binding.*](../binding/index.md) method group.

**Configurable activities.** The application customizes the appearance of the timeline entry through the `layout` parameter. To retrieve such an activity, use [crm.activity.configurable.get](../configurable/crm-activity-configurable-get.md). Retrieve other activities using the [crm.activity.get](./crm-activity-get.md) method.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [crm.activity.todo.add](../todo/crm-activity-todo-add.md) | Adds a universal activity to the timeline ||
|| [crm.activity.todo.update](../todo/crm-activity-todo-update.md) | Updates a universal activity ||
|| [crm.activity.get](./crm-activity-get.md) | Returns an activity by its identifier, except for configurable activities ||
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
