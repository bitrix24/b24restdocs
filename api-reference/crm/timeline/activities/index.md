# Activities in CRM: Overview of Methods

In CRM, activities are used for any tasks related to clients: calls, meetings, document approvals.

Activities are divided into incoming and scheduled:

* Incoming — activities that come from the client, such as an email, call, or chat. For these activities, it's important to correctly set the parameter `DIRECTION` = `1` so that the incoming activity counter in CRM works.

* Scheduled — activities created by employees, such as tasks or universal activities. They can have a deadline, include links to CRM entities, integrate with the calendar, invite colleagues, and attach files.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [activities in CRM](https://helpdesk.bitrix24.com/open/21648890/), [Activity Direction](../../auxiliary/enum/crm-enum-activity-direction.md) 

## Links of Activities with Other CRM Entities

Activities linked to CRM entities are stored in the timeline of the entity's card. If an activity is linked to multiple entities — for example, an email can be linked to both a deal and a contact — it will be stored in the timelines of all related entities.

Links between activities and CRM entities can be added and removed using the methods from the group [crm.activity.binding.*](./binding/index.md).

## System Activities

System activities in CRM are created automatically:

* A call activity is created by the connected telephony in Bitrix. To finish a call, use the method [telephony.externalcall.finish](../../../telephony/telephony-external-call-finish.md). This method ends the call, creates an activity in the entity's card, and returns the ID of the created activity in the parameter `CRM_ACTIVITY_ID`.

* An email activity is created by the email service. When an email from a client arrives at the connected Bitrix24 address, CRM checks if there is a client in the database with the email from the message. Based on the results, an activity will be created in the card of the found entity or a new client will be created, where the activity will appear.

To create, modify, or delete a system activity, use the group of methods [crm.activity.*](./crm-activity-add.md). When creating a system activity, specify `TYPE_ID`, for example, for an email activity `TYPE_ID` = `2`. To get values for other types of activities, use the method [crm.enum.activitytype](../../auxiliary/enum/crm-enum-activity-type.md).

### Custom Activity Types

Applications can register custom activity types: upload a custom icon and specify the type name. For example, you can create your own activity type with an icon and name of your application.

* To register an activity type — use the methods from the group [crm.activity.type.*](./types/index.md). When creating a type, you need to set its code designation in the parameter `TYPE_ID`.

* To create an activity with the application type — use the group of system activity methods [crm.activity.*](./crm-activity-add.md). When creating an activity, specify the code designation of the custom type `TYPE_ID`, registered for the activity type, in the parameter `PROVIDER_TYPE_ID`.

{% note tip "" %}

The methods [crm.activity.delete](./crm-activity-delete.md) (deletes an activity) and [crm.activity.list](./crm-activity-list.md) (retrieves a list of activities) are common for all types of CRM activities.

{% endnote %}

## Universal Activities

Universal activities are a type of activity with extended settings. In the card of a universal activity, you can synchronize the activity with the calendar, choose a meeting location with the client, add colleagues, select a client from a CRM entity, categorize activities by color, and choose a meeting room. Extended settings are available to employees on the Bitrix24 side.

To create a universal activity, use the method [crm.activity.todo.add](./crm-activity-todo-add.md). To change the deadline of an activity — use the method [crm.activity.todo.updateDeadline](./todo-update/crm-activity-todo-update-deadline.md), and to change the description of the activity — [crm.activity.todo.updateDescription](./todo-update/crm-activity-todo-update-description.md).

{% note tip "User Documentation" %}

  - [Universal Activity in Bitrix24](https://helpdesk.bitrix24.com/open/21458972/)

{% endnote %}

## Configurable Activities

Configurable activities are a type of activity that can only be created from an application. For this type, you can customize the appearance of the activity card and its functionality:

* [Structure of Configurable Activity](./structure/layout.md)
* [Badges of Configurable Activity](./badges/index.md)

To create or modify a configurable activity, use the group of methods [crm.activity.configurable.*](./crm-activity-configurable-add.md).

## Widgets

Applications can be embedded into activities. For embedding, special places are used, and there is one available in activities — [Context Menu Item of the Activity in the Entity Card](../../../widgets/crm/activity-timeline-menu.md) `CRM_XXX_ACTIVITY_TIMELINE_MENU`.

Thanks to the embedding, you can use the application without leaving the entity card. The application will open on the page you specify during the registration of the embedding.

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../../widgets/index.md)
- [Create Activities from Applications](./app-embedding/activity-app.md)

{% endnote %}

## Additional Features

**Text notes** can be added to activities and deleted. Use the group of methods [crm.timeline.note.*](../note/index.md).

**Content blocks** can be added to activities and deleted. Use the group of methods [crm.activity.layout.blocks.*](./layout-blocks/index.md).

* [Available Content Blocks](./structure/body.md#contentblockdto)

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can perform methods: any user

### General Methods and Events

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.activity.add](./crm-activity-add.md) | Creates a new activity ||
    || [crm.activity.update](./crm-activity-update.md) | Updates an activity ||
    || [crm.activity.get](./crm-activity-get.md) | Returns an activity by ID ||
    || [crm.activity.list](./crm-activity-list.md) | Returns a list of activities of all types by filter ||
    || [crm.activity.delete](./crm-activity-delete.md) | Deletes any type of activity ||
    || [crm.activity.fields](./crm-activity-fields.md) | Returns the description of activity fields ||
    || [crm.activity.communication.fields](./crm-activity-communication-fields.md) | Returns the description of communication fields ||
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

#|
|| **Method** | **Description** ||
|| [crm.activity.binding.add](./binding/crm-activity-binding-add.md) | Adds a binding ||
|| [crm.activity.binding.list](./binding/crm-activity-binding-list.md) | Returns a list of bindings ||
|| [crm.activity.binding.delete](./binding/crm-activity-binding-delete.md) | Deletes a binding ||
|#

### Custom Activity Types

#|
|| **Method** | **Description** ||
|| [crm.activity.type.add](./types/crm-activity-type-add.md) | Registers a new subtype of activity ||
|| [crm.activity.type.list](./types/crm-activity-type-list.md) | Returns a list of activity subtypes ||
|| [crm.activity.type.delete](./types/crm-activity-type-delete.md) | Deletes an activity subtype ||
|#

### Universal Activity

#|
|| **Method** | **Description** ||
|| [crm.activity.todo.add](./crm-activity-todo-add.md) | Creates a universal activity ||
|| [crm.activity.todo.updateDeadline](./todo-update/crm-activity-todo-update-deadline.md) | Changes the deadline ||
|| [crm.activity.todo.updateDescription](./todo-update/crm-activity-todo-update-description.md) | Changes the description ||
|#

### Configurable Activity

#|
|| **Method** | **Description** ||
|| [crm.activity.configurable.add](./crm-activity-configurable-add.md) | Creates a configurable activity ||
|| [crm.activity.configurable.update](./crm-activity-configurable-update.md) | Modifies a configurable activity ||
|| [crm.activity.configurable.get](./crm-activity-configurable-get.md) | Returns information about an activity by ID ||
|#

### Badges of Configurable Activity

#|
|| **Method** | **Description** ||
|| [crm.activity.badge.add](./badges/crm-activity-badge-add.md) | Creates a badge ||
|| [crm.activity.badge.get](./badges/crm-activity-badge-get.md) | Returns information about a badge ||
|| [crm.activity.badge.list](./badges/crm-activity-badge-list.md) | Returns a list of all registered badges ||
|| [crm.activity.badge.delete](./badges/crm-activity-badge-delete.md) | Deletes a badge ||
|#

### Additional Content Blocks

#|
|| **Method** | **Description** ||
|| [crm.activity.layout.blocks.set](./layout-blocks/crm-activity-layout-blocks-set.md) | Sets a set of additional content blocks in an activity ||
|| [crm.activity.layout.blocks.get](./layout-blocks/crm-activity-layout-blocks-get.md) | Retrieves the set of additional content blocks in the activity set by the application ||
|| [crm.activity.layout.blocks.delete](./layout-blocks/crm-activity-layout-blocks-delete.md) | Deletes the set of additional content blocks for the activity set by the application ||
|#