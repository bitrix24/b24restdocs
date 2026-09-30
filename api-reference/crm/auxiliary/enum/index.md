# Enumerations: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Enumeration methods return the numeric IDs that other CRM methods accept instead of names. For example, a legal address is type `6`, and a deal is object type `2`. These IDs are passed in the filters and parameters of address methods, activity methods, and universal methods.

> Quick navigation: [all methods](#all-methods)

## How to Work with Enumeration Methods

Enumerations are requested without parameters. The [crm.enum.ownertype](./crm-enum-owner-type.md), [crm.enum.addresstype](./crm-enum-address-type.md), and [crm.enum.settings.mode](./crm-enum-settings-mode.md) methods return an array of items with the fields `ID`, `NAME`, `SYMBOL_CODE`, and `SYMBOL_CODE_SHORT`. The [crm.enum.fields](./crm-enum-fields.md) method describes these fields. If an enumeration has no symbolic codes, the `SYMBOL_CODE` and `SYMBOL_CODE_SHORT` fields contain `null`, as with address types:

```json
{
    "ID": 6,
    "NAME": "Legal address",
    "SYMBOL_CODE": null,
    "SYMBOL_CODE_SHORT": null
}
```

The [crm.enum.getorderownertypes](./crm-enum-get-order-owner-types.md) method uses a different format — the fields `id`, `name`, `code`, and `attribute`:

```json
{
    "attribute": "DYN",
    "code": "DEAL",
    "id": 2,
    "name": "Deal"
}
```

Pass the identifier you receive to the parameter of the method you requested the enumeration for. For example, to retrieve the legal addresses of a contact:

1. retrieve the ID of the "Legal address" type with the [crm.enum.addresstype](./crm-enum-address-type.md) method, which is `6`

2. call the [crm.address.list](../../requisites/addresses/crm-address-list.md) method with the filter `TYPE_ID: 6`, `ANCHOR_TYPE_ID: 3` (contact), and `ANCHOR_ID` set to the contact ID. Without `ANCHOR_ID`, the selection includes the legal addresses of all contacts

## Relationship of Enumeration Methods with CRM Objects

**CRM Object.** The method [crm.enum.ownertype](./crm-enum-owner-type.md) returns identifiers for object types. Pass the `ID` of the object type in the `entityTypeId` parameter of the universal methods [crm.item.*](../../universal/index.md) and in the `OWNER_TYPE_ID` or `ownerTypeId` parameter of the [activity](../../timeline/activities/index.md) methods. Universal methods do not support the old invoice with ID `5` and requisites with ID `8`.

{% note tip "Typical use-cases and scenarios" %}

- [How to attach a task to an SPA](../../../../tutorials/tasks/how-to-connect-task-to-spa.md)

{% endnote %}

**Order.** The method [crm.enum.getorderownertypes](./crm-enum-get-order-owner-types.md) returns object types to which an order can be linked. Use the `id` of the object type in the `ownerTypeId` parameter value of the [crm.orderentity.add](../../universal/order-entity/crm-order-entity-add.md) method.

**Address.** The method [crm.enum.addresstype](./crm-enum-address-type.md) returns types of addresses. Use the `ID` of the address type in the `TYPE_ID` parameter value of the methods [crm.address.*](../../requisites/addresses/index.md).

{% note tip "Typical use-cases and scenarios" %}

- [How to get a client's address from CRM](../../../../tutorials/crm/how-to-get-lists/how-to-get-address.md)

{% endnote %}

**CRM Operating Mode.** The method [crm.enum.settings.mode](./crm-enum-settings-mode.md) returns the list of CRM operating modes. Use it to decode the number of the current mode returned by the method [crm.settings.mode.get](../../crm-settings-mode-get.md): `1` — Classic CRM, `2` — Simple CRM without leads.

### Activity Enumerations

Activity enumerations are deprecated and no longer being developed. The values of all six enumerations are listed in the [CRM data types](../../data-types.md#activity-enums) reference, and working with activities is covered in the [Activities in CRM](../../timeline/activities/index.md) section.

These values are accepted by the [activity](../../timeline/activities/index.md) methods:

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
> Who can execute the methods: a user with read access to leads, deals, or other CRM objects, including those in digital workspaces. The [crm.enum.getorderownertypes](./crm-enum-get-order-owner-types.md) method can be called by any user

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
