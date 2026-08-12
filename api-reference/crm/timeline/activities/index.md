# Activities in CRM: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In CRM, activities are used for any tasks related to customers: calls, meetings, or document approvals.

Activities are divided into incoming and scheduled:

* Incoming — activities that come from the customer, such as an e-mail, call, or chat. For these activities, it is important to correctly specify the parameter `DIRECTION` = `1` so that the incoming activities counter in CRM works.
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
|| [Activity of a Custom Type](./types/index.md) | An application via the [crm.activity.add](./activity-base/crm-activity-add.md) method once the type is registered | You need your own icon and activity type name in the interface ||
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

The `DIRECTION` parameter defines the direction of the activity: `1` — incoming, `2` — outgoing. Direction is relevant for calls and e-mails; it is not used for meetings.

## How to Get Started

1. Identify the CRM entity whose timeline will store the activity: the object type is passed in `OWNER_TYPE_ID`, and the identifier in `OWNER_ID`. You can find the type values in the [CRM Object Types](../../data-types.md#object_type) reference.
2. Retrieve the list of available fields using the [crm.activity.fields](./activity-base/crm-activity-fields.md) method.
3. Create the activity with the method for the kind you need: [crm.activity.add](./activity-base/crm-activity-add.md), [crm.activity.todo.add](./todo/crm-activity-todo-add.md), or [crm.activity.configurable.add](./configurable/crm-activity-configurable-add.md).
4. Retrieve the activity using the [crm.activity.get](./activity-base/crm-activity-get.md) method, or the list of activities of the entity using the [crm.activity.list](./activity-base/crm-activity-list.md) method.
5. Delete an activity you no longer need using the [crm.activity.delete](./activity-base/crm-activity-delete.md) method.
6. Subscribe to [activity events](./events/index.md) to track changes in real time.

## Links of Activities with Other CRM Entities

Activities linked to CRM entities are stored in the timeline of the entity's card. If an activity is linked to multiple entities — for example, an e-mail can be linked to both a deal and a contact — it will be stored in the timelines of all related entities.

Links between activities and CRM entities can be added and removed using the methods from the [crm.activity.binding.*](./binding/index.md) group.

## System Activities

System activities in CRM are created automatically:

* A call activity is created by the telephony connected in Bitrix24. To finish a call, use the method [telephony.externalcall.finish](../../../telephony/telephony-external-call-finish.md). This method ends the call, creates an activity in the entity's card, and returns the identifier of the created activity in the parameter `CRM_ACTIVITY_ID`.
* An e-mail activity is created by the e-mail system. When an e-mail from a customer arrives at the connected Bitrix24 address, CRM checks if there is a customer in the database with the e-mail from the message. Based on the results of the check, an activity will be created in the card of the found entity or a new customer, where the activity will appear.

To create, modify, or delete a system activity, use the methods from the [crm.activity.*](./activity-base/index.md) group. When creating a system activity, specify `TYPE_ID`, for example, `TYPE_ID` = `4` for an e-mail activity. The values of the other types are listed in the [Activity Type Identifiers](#activity-types) section.

### Activities of Custom Types

Applications can register custom activity types: upload a custom icon and specify the type name. For example, you can create your own activity type with an icon and name of your application.

* To register an activity type — use the methods from the [crm.activity.type.*](./types/index.md) group. When creating a type, you need to specify its code designation in the parameter `TYPE_ID`.
* To create an activity with the application type — use the group of system activity methods [crm.activity.add](./activity-base/crm-activity-add.md). When creating an activity, specify the code designation of the custom type `TYPE_ID`, registered for the activity type, in the parameter `PROVIDER_TYPE_ID`.

{% note tip "" %}

The methods [crm.activity.delete](./activity-base/crm-activity-delete.md) (deletes an activity) and [crm.activity.list](./activity-base/crm-activity-list.md) (retrieves a list of activities) are common for all types of CRM activities.

{% endnote %}

## Universal Activities

Universal activities are a type of activity with extended settings: a deadline, color, participants, a meeting room, and calendar synchronization.

The [crm.activity.todo.add](./todo/crm-activity-todo-add.md) method creates such an activity, and [crm.activity.todo.update](./todo/crm-activity-todo-update.md) updates it. Separate methods change a single property of the activity: the deadline, description, color, or responsible user. How to choose the right method is described in the [Universal CRM Activities](./todo/index.md) section.

## Configurable Activities

Configurable activities are a type of activity that can only be created from an application. For this type, you can customize the appearance of the activity card and its functionality:

* [Structure of Configurable Activity](./configurable/structure/layout.md)
* [Badges of Configurable Activity](./configurable/badges/index.md)

To create or modify a configurable activity, use the methods from the [crm.activity.configurable.*](./configurable/index.md) group.

## Widgets

Applications can be embedded into activities. For embedding, special locations are used, and one is available in activities — [Context Menu Item of the Activity in the Entity Card](../../../widgets/crm/activity-timeline-menu.md) `CRM_XXX_ACTIVITY_TIMELINE_MENU`.

Thanks to embedding, you can use the application without leaving the entity card. The application will open on the page you specify during the registration of the embedding.

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../../widgets/index.md)
- [Create Activities from Applications](./app-embedding/activity-app.md)

{% endnote %}

## Additional Features

**Text notes** can be added to activities and deleted. Use the methods from the [crm.timeline.note.*](../note/index.md) group.

**Content blocks** can be added to activities and deleted. Use the methods from the [crm.activity.layout.blocks.*](./layout-blocks/index.md).

* [Available Content Blocks](./configurable/structure/content-block.md)

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

### General Methods and Events

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.activity.add](./activity-base/crm-activity-add.md) | Creates a new activity ||
    || [crm.activity.update](./activity-base/crm-activity-update.md) | Updates an activity ||
    || [crm.activity.get](./activity-base/crm-activity-get.md) | Returns an activity by its identifier ||
    || [crm.activity.list](./activity-base/crm-activity-list.md) | Returns a list of activities of all types by filter ||
    || [crm.activity.delete](./activity-base/crm-activity-delete.md) | Deletes any type of activity ||
    || [crm.activity.call.getTranscript](./activity-base/crm-activity-call-get-transcript.md) | Returns a completed call transcription ||
    || [crm.activity.fields](./activity-base/crm-activity-fields.md) | Returns the description of activity fields ||
    || [crm.activity.communication.fields](./activity-base/crm-activity-communication-fields.md) | Returns the description of communication fields ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmActivityAdd](./events/on-crm-activity-add.md) | When an activity is created manually or via the [crm.activity.add](./activity-base/crm-activity-add.md) method ||
    || [onCrmActivityUpdate](./events/on-crm-activity-update.md) | When an activity is updated manually or via the [crm.activity.update](./activity-base/crm-activity-update.md) method ||
    || [onCrmActivityDelete](./events/on-crm-activity-delete.md) | When an activity is deleted manually or via the [crm.activity.delete](./activity-base/crm-activity-delete.md) method ||
    |#

{% endlist %}

### Managing Activity Links

#|
|| **Method** | **Description** ||
|| [crm.activity.binding.add](./binding/crm-activity-binding-add.md) | Adds a link between an activity and a CRM entity ||
|| [crm.activity.binding.list](./binding/crm-activity-binding-list.md) | Returns a list of links of an activity ||
|| [crm.activity.binding.move](./binding/crm-activity-binding-move.md) | Moves a link of an activity to another CRM entity ||
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
