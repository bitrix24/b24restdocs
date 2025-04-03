# Enumerations: Overview of Methods

Enumeration methods return information about the values of types: address type, deal type, object type, and others.

> Quick navigation: [all methods](#all-methods)

## How to Work with Enumeration Methods

Determine what data you need and select the enumeration method. For example, if you need to retrieve all legal addresses of a contact:

1. use the method [crm.enum.addresstype](./crm-enum-address-type.md) to find out the identifier for the legal address type

2. use the obtained identifier in the `TYPE_ID` filter parameter of the method [crm.address.list](../../requisites/addresses/crm-address-list.md)

## Relationship of Enumeration Methods with CRM Objects

**CRM Object.** The method [crm.enum.ownertype](./crm-enum-owner-type.md) returns the identifiers of object types. Use the `ID` of the object type in the `entityTypeId` parameter value of the methods [crm.item.*](../../universal/index.md), [crm.activity.*](../../timeline/activities/index.md).

{% note tip "Typical use-cases and scenarios" %}

- [How to attach a task to an SPA](../../../../tutorials/tasks/how-to-connect-task-to-spa.md)

{% endnote %}

**Order.** The method [crm.enum.getorderownertypes](./crm-enum-get-order-owner-types.md) returns the object types to which a connection with the order can be added. Use the `id` of the object type in the `ownerTypeId` parameter value of the methods [crm.orderentity.*](../../universal/order-entity/crm-order-entity-add.md).

**Description Type.** The method [crm.enum.contenttype](./crm-enum-content-type.md) returns the types of descriptions. Use the `ID` of the description type in the `DESCRIPTION_TYPE` parameter value of the methods [crm.activity.*](../../timeline/activities/index.md).

**Activity.** The method [crm.enum.activitytype](./crm-enum-activity-type.md) returns the types of activities. Use the `ID` of the activity type in the `TYPE_ID` parameter value of the methods [crm.activity.*](../../timeline/activities/index.md).

**Status.** The method [crm.enum.activitystatus](./crm-enum-activity-status.md) returns the types of activity statuses. Use the `ID` of the activity status in the `STATUS` parameter value of the methods [crm.activity.*](../../timeline/activities/index.md).

**Priority.** The method [crm.enum.activitypriority](./crm-enum-activity-priority.md) returns the types of activity priorities. Use the `ID` of the priority in the `PRIORITY` parameter value of the methods [crm.activity.*](../../timeline/activities/index.md).

**Direction.** The method [crm.enum.activitydirection](./crm-enum-activity-direction.md) returns the types of activity directions. Use the `ID` of the direction in the `DIRECTION` parameter value of the methods [crm.activity.*](../../timeline/activities/index.md).

{% note tip "Typical use-cases and scenarios" %}

- [How to send an e-mail to a client](../../../../tutorials/crm/how-to-add-crm-objects/how-to-send-email.md)

{% endnote %}

**Notification.** The method [crm.enum.activitynotifytype](./crm-enum-activity-notify-type.md) returns the types of notifications for activities. Use the `ID` of the notification type in the `NOTIFY_TYPE` parameter value of the methods [crm.activity.*](../../timeline/activities/index.md).

**Address.** The method [crm.enum.addresstype](./crm-enum-address-type.md) returns the types of addresses. Use the `ID` of the address type in the `TYPE_ID` parameter value of the methods [crm.address.*](../../requisites/addresses/index.md).

{% note tip "Typical use-cases and scenarios" %}

- [How to get a client's address from CRM](../../../../tutorials/crm/how-to-get-lists/how-to-get-address.md)

{% endnote %}

**CRM Operating Mode.** The method [crm.enum.settings.mode](./crm-enum-settings-mode.md) returns the type of CRM. Use this method to decode the ID type value returned by the method [crm.settings.mode.get](../../crm-settings-mode-get.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.enum.fields](./crm-enum-fields.md) | Returns the description of enumeration fields ||
|| [crm.enum.ownertype](./crm-enum-owner-type.md) | Returns the enumeration items for "Owner Type" ||
|| [crm.enum.getorderownertypes](./crm-enum-get-order-owner-types.md) | Returns the identifiers of object types available for order binding ||
|| [crm.enum.contenttype](./crm-enum-content-type.md) | Returns the enumeration items for "Content Type" ||
|| [crm.enum.activitytype](./crm-enum-activity-type.md) | Returns the enumeration items for "Activity Type" ||
|| [crm.enum.activitypriority](./crm-enum-activity-priority.md) | Returns the enumeration items for "Activity Priority" ||
|| [crm.enum.activitydirection](./crm-enum-activity-direction.md) | Returns the enumeration items for "Activity Direction," for emails and calls ||
|| [crm.enum.activitynotifytype](./crm-enum-activity-notify-type.md) | Returns the enumeration items for "Activity Notification Type," for meetings and calls ||
|| [crm.enum.addresstype](./crm-enum-address-type.md) | Returns the enumeration items for "Address Type" ||
|| [crm.enum.activitystatus](./crm-enum-activity-status.md) | Returns the enumeration items for "Status" ||
|| [crm.enum.settings.mode](./crm-enum-settings-mode.md) | Returns the description of CRM operating modes ||
|#