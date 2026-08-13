# Enumerations: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Enumeration methods return information about the values of types: address type, activity type, object type, and others.

> Quick navigation: [all methods](#all-methods)

## How to Work with Enumeration Methods

Enumeration methods are called without parameters. The response contains an array of items with the fields `ID`, `NAME`, `SYMBOL_CODE`, and `SYMBOL_CODE_SHORT` — their description is returned by the method [crm.enum.fields](./crm-enum-fields.md). The method [crm.enum.getorderownertypes](./crm-enum-get-order-owner-types.md) uses a different format: the fields `id`, `name`, `code`, and `attribute`.

Pass the identifier you receive to the parameter of the method you requested the enumeration for.

Determine what data you need and select the enumeration method. For example, if you need to retrieve all legal addresses of a contact:

1. use the method [crm.enum.addresstype](./crm-enum-address-type.md) to find out the identifier for the legal address type

2. use the obtained identifier in the `TYPE_ID` filter parameter in the method [crm.address.list](../../requisites/addresses/crm-address-list.md)

## Relationship of Enumeration Methods with CRM Objects

**CRM Object.** The method [crm.enum.ownertype](./crm-enum-owner-type.md) returns identifiers for object types. Use the `ID` of the object type in the `entityTypeId` parameter value of the methods [crm.item.*](../../universal/index.md), [crm.activity.*](../../timeline/activities/index.md).

{% note tip "Typical use-cases and scenarios" %}

- [How to attach a task to an SPA](../../../../tutorials/tasks/how-to-connect-task-to-spa.md)

{% endnote %}

**Order.** The method [crm.enum.getorderownertypes](./crm-enum-get-order-owner-types.md) returns object types to which an order can be linked. Use the `id` of the object type in the `ownerTypeId` parameter value of the methods [crm.orderentity.*](../../universal/order-entity/crm-order-entity-add.md).

**Address.** The method [crm.enum.addresstype](./crm-enum-address-type.md) returns types of addresses. Use the `ID` of the address type in the `TYPE_ID` parameter value of the methods [crm.address.*](../../requisites/addresses/index.md).

{% note tip "Typical use-cases and scenarios" %}

- [How to get a client's address from CRM](../../../../tutorials/crm/how-to-get-lists/how-to-get-address.md)

{% endnote %}

**CRM Operating Mode.** The method [crm.enum.settings.mode](./crm-enum-settings-mode.md) returns the list of CRM operating modes. Use it to decode the `ID` value returned by the method [crm.settings.mode.get](../../crm-settings-mode-get.md).

### Activity Enumerations

Activity enumerations are deprecated and no longer being developed. Current activity types, statuses, and directions are described in the [Activities in CRM](../../timeline/activities/index.md) section.

The group of methods `crm.activity.*` uses the values of these enumerations:

- **Activity.** The method [crm.enum.activitytype](./outdated/crm-enum-activity-type.md) returns activity types for the `TYPE_ID` parameter
- **Status.** The method [crm.enum.activitystatus](./outdated/crm-enum-activity-status.md) returns activity statuses for the `STATUS` parameter
- **Priority.** The method [crm.enum.activitypriority](./outdated/crm-enum-activity-priority.md) returns activity priorities for the `PRIORITY` parameter
- **Direction.** The method [crm.enum.activitydirection](./outdated/crm-enum-activity-direction.md) returns activity directions for the `DIRECTION` parameter
- **Notification.** The method [crm.enum.activitynotifytype](./outdated/crm-enum-activity-notify-type.md) returns notification types for the `NOTIFY_TYPE` parameter
- **Description Type.** The method [crm.enum.contenttype](./outdated/crm-enum-content-type.md) returns description types for the `DESCRIPTION_TYPE` parameter

{% note tip "Typical use-cases and scenarios" %}

- [How to send an e-mail to a client](../../../../tutorials/crm/how-to-add-crm-objects/how-to-send-email.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform methods: any user

#|
|| **Method** | **Description** ||
|| [crm.enum.fields](./crm-enum-fields.md) | Returns descriptions of the fields of enumeration items ||
|| [crm.enum.getorderownertypes](./crm-enum-get-order-owner-types.md) | Returns identifiers of object types to which order binding is available ||
|| [crm.enum.ownertype](./crm-enum-owner-type.md) | Returns object types in CRM ||
|| [crm.enum.addresstype](./crm-enum-address-type.md) | Returns types of addresses ||
|| [crm.enum.settings.mode](./crm-enum-settings-mode.md) | Returns descriptions of CRM operating modes ||
|#

### Deprecated Methods

The methods below are no longer being developed.

#|
|| **Method** | **Description** ||
|| [crm.enum.activitytype](./outdated/crm-enum-activity-type.md) | Returns enumeration items "Activity Types" ||
|| [crm.enum.activitydirection](./outdated/crm-enum-activity-direction.md) | Returns enumeration items "Activity Direction" for e-mails and calls ||
|| [crm.enum.activitypriority](./outdated/crm-enum-activity-priority.md) | Returns enumeration items "Activity Priorities" ||
|| [crm.enum.activitynotifytype](./outdated/crm-enum-activity-notify-type.md) | Returns enumeration items "Notification Type for Activity Start" for meetings and calls ||
|| [crm.enum.contenttype](./outdated/crm-enum-content-type.md) | Returns enumeration items "Description Type" ||
|| [crm.enum.activitystatus](./outdated/crm-enum-activity-status.md) | Returns enumeration items "Status" ||
|#
