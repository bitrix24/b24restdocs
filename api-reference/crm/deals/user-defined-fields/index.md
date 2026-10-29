# Custom Fields for Deals: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Custom fields store information about a deal in various data formats: string, number, link, address, and others. A field is created once and appears in the detail forms of all deals in Bitrix24.

> Quick navigation: [all methods and events](#all-methods)
>
> User Documentation: [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## Custom Field Names

The field name is assembled by Bitrix24, not by the developer. The `UF_CRM_` prefix is always added to the code you pass in the `FIELD_NAME` parameter of the method [crm.deal.userfield.add](./crm-deal-userfield-add.md): the code `MANAGER_NOTE` becomes the name `UF_CRM_MANAGER_NOTE`. The complete conversion rules are described on the method page.

The resulting name is the key used to write and read the field value in deal methods, for example, [crm.deal.add](../crm-deal-add.md) and [crm.deal.list](../crm-deal-list.md). The method [crm.deal.fields](../crm-deal-fields.md) returns custom fields together with system ones.

## How to Get Started

1. Choose a field type. The method [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) returns the list of types with identifiers for the `USER_TYPE_ID` parameter, for example, `double` for a number. The method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md) describes the characteristics common to any field: whether it is required, whether it is multiple, the sort order.
2. Find out the type settings for the `SETTINGS` parameter with the method [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md), for example, `PRECISION` for the number precision. The method `crm.deal.userfield.add` does not accept all type settings. The supported settings are listed on the [method page](./crm-deal-userfield-add.md#settings).
3. Create the field with the method [crm.deal.userfield.add](./crm-deal-userfield-add.md). The method returns the field identifier.
4. Retrieve the field name with the `UF_CRM_` prefix using the method [crm.deal.userfield.get](./crm-deal-userfield-get.md) by the identifier, or find it in the response of [crm.deal.userfield.list](./crm-deal-userfield-list.md).
5. Write the value to a deal with the method [crm.deal.update](../crm-deal-update.md): `"fields": {"UF_CRM_MANAGER_NOTE": "Call back on Friday"}`. For an `enumeration` field, pass the `ID` of an item from the `LIST` array.
6. Read the value with the method [crm.deal.get](../crm-deal-get.md) — it arrives in the `UF_CRM_MANAGER_NOTE` field.

The `crm.deal.userfield.*` methods work only with deal fields. To manage fields of different objects with a single group of methods, use [userfieldconfig.*](../../universal/userfieldconfig/index.md). How to choose between the two groups of methods is described on the page [Custom Fields in CRM](../../universal/user-defined-fields/index.md).

## Errors When Working with Custom Fields {#errors}

When creating or deleting custom fields, the request may be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal server error. The cause can be found in the server logs at the time of the request.

* In the cloud Bitrix24, submit a request to [technical support](../../../../bitrix-support.md) to get details about the error.
* In the on-premise Bitrix24, request the server error log from the server administrator or hosting administrator, then contact [technical support](../../../../bitrix-support.md) and attach the log for analysis.

### Common Causes of Server Errors

1. You can create 1016 custom fields for deals — this is a limitation of the database architecture. If there are already 1016 fields for deals in Bitrix24, attempting to create a new field will result in the method [crm.deal.userfield.add](./crm-deal-userfield-add.md) returning the error `INTERNAL_SERVER_ERROR`.

    You can check the number of custom fields for deals using the method [crm.deal.userfield.list](./crm-deal-userfield-list.md).

2. There is a limitation on servers for the execution time of a single request — `max_execution_time`. The standard time is 60 seconds. If the request takes longer, it is interrupted with the error `INTERNAL_SERVER_ERROR`.

    The time for [creating](./crm-deal-userfield-add.md) or [deleting](./crm-deal-userfield-delete.md) a custom field for deals depends on the number of deals. When a field is created, it is added to all deal detail forms. When a field is deleted, it is removed from all detail forms. The fewer deals in Bitrix24, the faster fields are created and deleted.

    To check the number of deals, use the method [crm.deal.list](../crm-deal-list.md).

## Overview of Methods and Events {#all-methods}

The events notify about the configuration of the fields themselves, not about changes to their values in deals. How to subscribe to them is described on the page [events for custom deal fields](./events/index.md).

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method. A user with the "Allow to modify settings" access permission in CRM can create, modify, and delete a field. Any user with the "read" access permission for deals can retrieve a field and a list of fields. Any user can subscribe to the events

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [crm.deal.userfield.add](./crm-deal-userfield-add.md) | Creates a new custom field for deals ||
    || [crm.deal.userfield.update](./crm-deal-userfield-update.md) | Modifies an existing custom field for deals ||
    || [crm.deal.userfield.get](./crm-deal-userfield-get.md) | Returns a custom field for deals by its identifier ||
    || [crm.deal.userfield.list](./crm-deal-userfield-list.md) | Returns a list of custom fields for deals ||
    || [crm.deal.userfield.delete](./crm-deal-userfield-delete.md) | Deletes a custom field for deals ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [onCrmDealUserFieldAdd](./events/on-crm-deal-user-field-add.md) | When a custom field is added ||
    || [onCrmDealUserFieldUpdate](./events/on-crm-deal-user-field-update.md) | When a custom field is modified ||
    || [onCrmDealUserFieldDelete](./events/on-crm-deal-user-field-delete.md) | When a custom field is deleted ||
    || [onCrmDealUserFieldSetEnumValues](./events/on-crm-deal-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed ||
    |#

{% endlist %}
