# Activities in CRM: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

In CRM, activities are used for any tasks related to customers: calls, meetings, or document approvals.

Activities are divided into incoming and scheduled:

* Incoming — activities that come from the customer, such as an e-mail, call, or chat. For these activities, it is important to specify the direction correctly so that the incoming activities counter in CRM works.
* Scheduled — activities created by employees, such as tasks or universal activities. They can have a deadline, add links to CRM entities, integrate with the calendar, invite colleagues, and attach files.

> Quick navigation: [All Methods and Events](#all-methods)
>
> User documentation: [Activities in CRM](https://helpdesk.bitrix24.com/open/21648890/)

## Which Kind of Activity to Use

There are four kinds of activities in CRM. They differ in who creates the activity and how far its detail form can be customized.

#|
|| **Kind of Activity** | **Created By** | **When to Use** ||
|| [System Activity](./activity-base/index.md) | Telephony, e-mail, chat, or an application via the [crm.activity.add](./activity-base/crm-activity-add.md) method | You need a call, e-mail, meeting, or an activity with a standard detail form ||
|| [Universal Activity](./todo/index.md) | An employee in the entity detail form or an application via the [crm.activity.todo.add](./todo/crm-activity-todo-add.md) method | You need a deadline, color, participants, a meeting room, and calendar synchronization ||
|| [Configurable Activity](./configurable/index.md) | Only an application via the [crm.activity.configurable.add](./configurable/crm-activity-configurable-add.md) method | You need a custom appearance of the activity detail form with application blocks and buttons ||
|| [Activity of a Custom Type](./types/index.md) | An application once the type is registered: via the [crm.activity.add](./activity-base/crm-activity-add.md) method or, for a configurable type, via the [crm.activity.configurable.add](./configurable/crm-activity-configurable-add.md) method | You need your own icon and activity type name in the interface ||
|#

## Activity Type Identifiers {#activity-types}

The `TYPE_ID` parameter defines the type of a system activity.

#|
|| **TYPE_ID** | **Activity Type** ||
|| `1` | Meeting ||
|| `2` | Call ||
|| `3` | Task ||
|| `4` | E-mail ||
|| `5` | General activity, used when importing calendar events ||
|| `6` | Provider activity: universal and configurable activities, and application activities ||
|#

The value `0` in a response means that the type is not defined. Statuses, priority, direction, and other activity enumerations are described in the [Activity Enumerations](../../data-types.md#activity-enums) reference.

The `DIRECTION` parameter defines the direction of the activity: `1` — incoming, `2` — outgoing. Direction is relevant for calls and e-mails; it is not used for meetings.

## How to Get Started

1. Identify the CRM entity whose timeline will store the activity: the object type is passed in `OWNER_TYPE_ID`, and the identifier in `OWNER_ID`. You can find the type values in the [CRM Object Types](../../data-types.md#object_type) reference.
2. See the field descriptions in the [REST 3.0 Activity Fields](../../rest-v3/activities/index.md#fields) section. The `crm.activity.field.list` method returns field descriptions, including whether a field supports filtering and sorting.
3. Create the activity with the method for the kind you need: [crm.activity.todo.add](./todo/crm-activity-todo-add.md) for a universal activity, [crm.activity.configurable.add](./configurable/crm-activity-configurable-add.md) for a configurable one. For other kinds, use [crm.activity.add](./activity-base/crm-activity-add.md): REST 3.0 methods do not yet create activities or modify their fields. Update an activity with the method for its kind: [crm.activity.todo.update](./todo/crm-activity-todo-update.md), [crm.activity.configurable.update](./configurable/crm-activity-configurable-update.md), or [crm.activity.update](./activity-base/crm-activity-update.md).
4. Retrieve the activity using the REST 3.0 [crm.activity.get](../../rest-v3/activities/crm-activity-get-rest-v3.md) method, and the list of activities of an entity using the [crm.&lt;type&gt;.activity.list](../../rest-v3/activities/crm-entity-activity-list-rest-v3.md) method. The activity fields that REST 3.0 methods do not return are listed in the [Deprecated Methods](#deprecated) section.
5. Delete an activity you no longer need using the REST 3.0 [crm.activity.delete](../../rest-v3/activities/crm-activity-delete-rest-v3.md) method.
6. Subscribe to [activity events](./events/index.md) to track changes in real time.

## Links of Activities with Other CRM Entities {#bindings}

Activities linked to CRM entities are stored in the timeline of the entity's card. If an activity is linked to multiple entities — for example, an e-mail can be linked to both a deal and a contact — it is stored in the timelines of all related entities.

The REST 3.0 methods [crm.activity.binding.bind](../../rest-v3/activities/crm-activity-binding-bind-rest-v3.md), [crm.activity.binding.move](../../rest-v3/activities/crm-activity-binding-move-rest-v3.md), and [crm.activity.binding.unbind](../../rest-v3/activities/crm-activity-binding-unbind-rest-v3.md) add, move, and remove links between activities and CRM entities. The [crm.activity.binding.list](../../rest-v3/activities/crm-activity-binding-list-rest-v3.md) method returns the list of links.

## System Activities

Some system activities in CRM are created automatically:

* A call activity is created by the telephony connected in Bitrix24. To finish a call, use the method [telephony.externalcall.finish](../../../telephony/telephony-external-call-finish.md). This method ends the call, creates an activity in the entity's card, and returns the identifier of the created activity in the parameter `CRM_ACTIVITY_ID`.
* An e-mail activity is created by the e-mail system. When an e-mail from a customer arrives at the connected Bitrix24 address, CRM checks if there is a customer in the database with the e-mail from the message. If the customer is found, the activity appears in their card. If not, CRM creates a new customer and adds the activity to their card.

To create, modify, or delete a system activity, use the methods from the [crm.activity.*](./activity-base/index.md) group. The `TYPE_ID` values are listed in the [Activity Type Identifiers](#activity-types) section.

### Activities of Custom Types

Applications can register custom activity types with their own icon and name.

* To register an activity type — use the methods from the [crm.activity.type.*](./types/index.md) group. When creating a type, you need to specify its code designation in the parameter `TYPE_ID`.
* To create an activity with the application type — use the [crm.activity.add](./activity-base/crm-activity-add.md) method: pass the type code in `PROVIDER_TYPE_ID` and `PROVIDER_ID` = `REST_APP`. If the type is registered with `IS_CONFIGURABLE_TYPE` = `Y`, pass its code in the `typeId` field of the [crm.activity.configurable.add](./configurable/crm-activity-configurable-add.md) method.

## Universal Activities

In addition to [crm.activity.todo.add](./todo/crm-activity-todo-add.md) and [crm.activity.todo.update](./todo/crm-activity-todo-update.md), universal activities have methods that change a single property of the activity: the deadline, description, color, or responsible user. How to choose the right method is described in the [Universal CRM Activities](./todo/index.md) section.

## Configurable Activities

An application customizes the appearance and functionality of the configurable activity card:

* [Structure of Configurable Activity](./configurable/structure/layout.md)
* [Badges of Configurable Activity](./configurable/badges/index.md)

## Widgets

Applications can be embedded into activities. Two widget placements are available in activities. [Context Menu Item in the Activity List](../../../widgets/crm/list-menu.md) — `CRM_ACTIVITY_LIST_MENU`. [Context Menu Item of the Activity in the Entity Card](../../../widgets/crm/activity-timeline-menu.md) — `CRM_XXX_ACTIVITY_TIMELINE_MENU`, where `XXX` is replaced with the object type, for example `DEAL`. For SPAs — `CRM_DYNAMIC_XXX_ACTIVITY_TIMELINE_MENU`, where `XXX` is the numeric SPA type identifier, for example `CRM_DYNAMIC_183_ACTIVITY_TIMELINE_MENU`.

Widgets let you work with the application without leaving CRM. The application opens on the page you specify when registering the widget.

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../../widgets/index.md)
- [Create Activities from Applications](./app-embedding/activity-app.md)

{% endnote %}

## Additional Features

**Text notes** can be added to activities and deleted. Use the methods from the [crm.timeline.note.*](../note/index.md) group.

**Content blocks** can be added to activities and deleted. Use the methods from the [crm.activity.layout.blocks.*](./layout-blocks/index.md).

* [Available Content Blocks](./configurable/structure/content-block.md)

## Deprecated Methods {#deprecated}

Methods of the previous version still work, but their development has been discontinued. For new development, use the replacements from the table.

#|
|| **Method** | **Replacement** | **Replacement Limitations** ||
|| [crm.activity.add](./activity-base/crm-activity-add.md) | [crm.activity.todo.add](./todo/crm-activity-todo-add.md) | Creates only a universal activity. Calls, meetings, e-mails, and activities of a custom type are still created by `crm.activity.add` ||
|| [crm.activity.update](./activity-base/crm-activity-update.md) | [crm.activity.todo.update](./todo/crm-activity-todo-update.md) | Updates only a universal activity ||
|| [crm.activity.get](./activity-base/crm-activity-get.md) | REST 3.0 [crm.activity.get](../../rest-v3/activities/crm-activity-get-rest-v3.md) | Does not return some fields, for example `settings`, `providerParams`, `location`, `status` ||
|| [crm.activity.list](./activity-base/crm-activity-list.md) | REST 3.0 [crm.activity.list](../../rest-v3/activities/crm-activity-list-rest-v3.md) | Available only to a Bitrix24 administrator and does not return some fields. To retrieve the activities of a single entity without administrator rights, use [crm.&lt;type&gt;.activity.list](../../rest-v3/activities/crm-entity-activity-list-rest-v3.md) ||
|| [crm.activity.delete](./activity-base/crm-activity-delete.md) | REST 3.0 [crm.activity.delete](../../rest-v3/activities/crm-activity-delete-rest-v3.md) | Requires permission to delete in the CRM entities the activity is linked to ||
|| [crm.activity.fields](./activity-base/crm-activity-fields.md), [crm.activity.communication.fields](./activity-base/crm-activity-communication-fields.md) | REST 3.0 `crm.activity.field.list`, described in the [Activity Fields](../../rest-v3/activities/index.md#fields) section | Does not describe some fields, for example `settings` and `providerParams`. Communications are described by the `communications` field ||
|| [crm.activity.binding.*](./binding/index.md) | REST 3.0 [crm.activity.binding.bind](../../rest-v3/activities/crm-activity-binding-bind-rest-v3.md), [unbind](../../rest-v3/activities/crm-activity-binding-unbind-rest-v3.md), [move](../../rest-v3/activities/crm-activity-binding-move-rest-v3.md), [list](../../rest-v3/activities/crm-activity-binding-list-rest-v3.md) | Parameters and responses follow the REST 3.0 format and are described on the method pages ||
|#

Other REST 3.0 activity methods — completing [crm.activity.complete](../../rest-v3/activities/crm-activity-complete-rest-v3.md), returning to work [crm.activity.uncomplete](../../rest-v3/activities/crm-activity-uncomplete-rest-v3.md), entity methods `crm.<type>.activity.*`, and e-mail methods `crm.activity.mail.*` — are listed in the [REST 3.0 activities overview](../../rest-v3/activities/index.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

### General Methods and Events

{% list tabs %}

- Methods

    Development of these methods, except `crm.activity.call.getTranscript`, has been discontinued. Replacements are listed in the [Deprecated Methods](#deprecated) section.

    #|
    || **Method** | **Description** ||
    || [crm.activity.add](./activity-base/crm-activity-add.md) | Creates a new activity ||
    || [crm.activity.update](./activity-base/crm-activity-update.md) | Updates an activity ||
    || [crm.activity.get](./activity-base/crm-activity-get.md) | Returns an activity by its identifier ||
    || [crm.activity.list](./activity-base/crm-activity-list.md) | Returns a list of activities of all types by filter ||
    || [crm.activity.delete](./activity-base/crm-activity-delete.md) | Deletes an activity of any kind ||
    || [crm.activity.call.getTranscript](./activity-base/crm-activity-call-get-transcript.md) | Returns a completed call transcription ||
    || [crm.activity.fields](./activity-base/crm-activity-fields.md) | Returns the description of activity fields ||
    || [crm.activity.communication.fields](./activity-base/crm-activity-communication-fields.md) | Returns the description of communication fields ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmActivityAdd](./events/on-crm-activity-add.md) | When an activity is created ||
    || [onCrmActivityUpdate](./events/on-crm-activity-update.md) | When an activity is updated ||
    || [onCrmActivityDelete](./events/on-crm-activity-delete.md) | When an activity is deleted ||
    |#

{% endlist %}

### Managing Activity Links

Development of these methods has been discontinued. Replacements are listed in the [Deprecated Methods](#deprecated) section.

#|
|| **Method** | **Description** ||
|| [crm.activity.binding.add](./binding/crm-activity-binding-add.md) | Adds a link between an activity and a CRM entity ||
|| [crm.activity.binding.move](./binding/crm-activity-binding-move.md) | Moves a link of an activity to another CRM entity ||
|| [crm.activity.binding.list](./binding/crm-activity-binding-list.md) | Returns a list of links of an activity ||
|| [crm.activity.binding.delete](./binding/crm-activity-binding-delete.md) | Deletes a link between an activity and a CRM entity ||
|#

### Custom Activity Types

#|
|| **Method** | **Description** ||
|| [crm.activity.type.add](./types/crm-activity-type-add.md) | Registers a custom activity type with a name and icon ||
|| [crm.activity.type.list](./types/crm-activity-type-list.md) | Retrieves a list of custom activity types ||
|| [crm.activity.type.delete](./types/crm-activity-type-delete.md) | Deletes a custom activity type ||
|#

### Universal Activity

#|
|| **Method** | **Description** ||
|| [crm.activity.todo.add](./todo/crm-activity-todo-add.md) | Creates a universal activity ||
|| [crm.activity.todo.update](./todo/crm-activity-todo-update.md) | Updates a universal activity ||
|| [crm.activity.todo.updateColor](./todo/crm-activity-todo-update-color.md) | Changes the color ||
|| [crm.activity.todo.updateDeadline](./todo/crm-activity-todo-update-deadline.md) | Changes the deadline ||
|| [crm.activity.todo.updateDescription](./todo/crm-activity-todo-update-description.md) | Changes the description ||
|| [crm.activity.todo.updateResponsibleUser](./todo/crm-activity-todo-update-responsible-user.md) | Changes the responsible user ||
|#

### Configurable Activity

#|
|| **Method** | **Description** ||
|| [crm.activity.configurable.add](./configurable/crm-activity-configurable-add.md) | Adds a new configurable activity to the timeline ||
|| [crm.activity.configurable.update](./configurable/crm-activity-configurable-update.md) | Updates a configurable activity ||
|| [crm.activity.configurable.get](./configurable/crm-activity-configurable-get.md) | Retrieves information about an activity by ID ||
|#

### Badges of Configurable Activity

#|
|| **Method** | **Description** ||
|| [crm.activity.badge.add](./configurable/badges/crm-activity-badge-add.md) | Adds a new badge ||
|| [crm.activity.badge.get](./configurable/badges/crm-activity-badge-get.md) | Retrieves information about a badge ||
|| [crm.activity.badge.list](./configurable/badges/crm-activity-badge-list.md) | Retrieves a list of badges ||
|| [crm.activity.badge.delete](./configurable/badges/crm-activity-badge-delete.md) | Deletes a badge by code ||
|#

### Additional Content Blocks

#|
|| **Method** | **Description** ||
|| [crm.activity.layout.blocks.set](./layout-blocks/crm-activity-layout-blocks-set.md) | Sets a set of additional content blocks in the activity ||
|| [crm.activity.layout.blocks.get](./layout-blocks/crm-activity-layout-blocks-get.md) | Retrieves the set of additional content blocks in the activity set by the application ||
|| [crm.activity.layout.blocks.delete](./layout-blocks/crm-activity-layout-blocks-delete.md) | Deletes the set of additional content blocks for the activity set by the application ||
|#
