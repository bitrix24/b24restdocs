# Custom Fields for Estimates: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Custom fields add data to the estimate detail form for which there are no system fields, for example, a manager's note. A field can store a string, a number, a link, an address, or a reference to another CRM object. Estimates themselves are created and updated by the methods described in the [estimates overview](../index.md).

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## Getting Started

1. Choose the field type: it determines what data the field stores and which settings it has. The method [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) returns all types
2. Find out the settings of the chosen type using the method [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md). The method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md) returns the general field parameters
3. Create the field using the method [crm.quote.userfield.add](./crm-quote-user-field-add.md). The response contains the field identifier. The method [crm.quote.userfield.get](./crm-quote-user-field-get.md#result) returns the full name, type, settings, and labels of the field
4. Write and read the field value using the methods [crm.item.add](../../universal/crm-item-add.md), [crm.item.update](../../universal/crm-item-update.md), and [crm.item.get](../../universal/crm-item-get.md) with `entityTypeId = 7`. For example, `"fields": {"ufCrmManagerNote": "Call back on Friday"}` — how the field name changes in these methods is described in the "Custom Field Name" section. For an `enumeration` field, pass the `ID` of an item from the `LIST` array in the response of [crm.quote.userfield.get](./crm-quote-user-field-get.md#result)

The `crm.quote.userfield.*` methods work only with estimate fields. Custom fields of any CRM object are managed by the [userfieldconfig.*](../../universal/userfieldconfig/index.md) methods. In REST 3.0, estimate fields are configured by the `crm.quote.field.custom.*` methods — they are described in the [REST 3.0 fields overview](../../rest-v3/fields/index.md).

## Custom Field Name

The field code is set in the required parameter `FIELD_NAME` of the method [crm.quote.userfield.add](./crm-quote-user-field-add.md). Bitrix24 builds the full name itself: it adds the `UF_CRM_` prefix to the code. `MANAGER_NOTE` becomes `UF_CRM_MANAGER_NOTE`. For fields created in the Bitrix24 interface, the code is substituted automatically, and their names look like `UF_CRM_QUOTE_1768404002`.

The full name is needed in three places:

- in the `fields` parameter of the methods [crm.quote.add](../crm-quote-add.md) and [crm.quote.update](../crm-quote-update.md) — to write the value. The development of these methods has been halted
- in the `select` parameter of the method [crm.quote.list](../crm-quote-list.md) — to return the value in the selection. The development of this method has also been halted. The `UF_*` mask selects all custom fields at once
- in the `FIELD_NAME` field of custom field [events](./events/index.md) — to determine which field the event is about

In the universal `crm.item.*` methods with `entityTypeId = 7`, the same field is named in camelCase: `UF_CRM_MANAGER_NOTE` becomes `ufCrmManagerNote`. These methods return the original name if you pass the parameter `useOriginalUfNames = Y`.

## Types of Custom Fields

The type is set in the `USER_TYPE_ID` parameter when the field is created. It cannot be changed afterwards.

#|
|| **Type** | **What It Stores** ||
|| `string` | A string ||
|| `integer` | An integer ||
|| `double` | A floating-point number ||
|| `boolean` | A yes or no value ||
|| `date` | A date ||
|| `datetime` | A date and time ||
|| `money` | An amount with a currency ||
|| `url` | A link ||
|| `address` | An address on a map ||
|| `file` | A file ||
|| `enumeration` | A value from a list that is defined in the `LIST` parameter ||
|| `employee` | A reference to a Bitrix24 user ||
|| `crm` | A reference to a CRM entity: a lead, deal, contact, or company ||
|| `crm_status` | A reference to a CRM reference guide item ||
|| `iblock_element` | A reference to an information block element ||
|| `iblock_section` | A reference to an information block section ||
|#

## Errors When Working with Custom Fields {#errors}

When creating or deleting a custom field, the request may be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal error: its cause is visible only in the server logs for the time of the request. How to get the error details:

- in cloud Bitrix24, submit a request to [technical support](../../../../bitrix-support.md)
- in on-premise Bitrix24, request the server error log from the server or hosting administrator, and then contact [technical support](../../../../bitrix-support.md) and attach the log for analysis

### Common Causes of Server Errors

1. You can create 1016 custom fields for estimates — this is a limitation of the database architecture. If there are already 1016 fields for estimates in Bitrix24, attempting to create a new field will result in the method [crm.quote.userfield.add](./crm-quote-user-field-add.md) returning an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md)

    You can check the number of custom fields for estimates using the method [crm.quote.userfield.list](./crm-quote-user-field-list.md)

2. The server interrupts a request that takes longer than `max_execution_time` and returns an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). The standard value of `max_execution_time` is 60 seconds

    [Creating](./crm-quote-user-field-add.md) and [deleting](./crm-quote-user-field-delete.md) a field take longer the more estimates there are in Bitrix24: the field is added to all estimate detail forms or removed from them

    You can find out the number of estimates using the method [crm.quote.list](../crm-quote-list.md)

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method. A user with the "Edit CRM preferences" permission in CRM can create, update, and delete a field. A user with permission to read estimates can retrieve a field and a list of fields

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [crm.quote.userfield.add](./crm-quote-user-field-add.md) | Creates a new custom field for estimates ||
    || [crm.quote.userfield.update](./crm-quote-user-field-update.md) | Updates an existing custom field for estimates ||
    || [crm.quote.userfield.get](./crm-quote-user-field-get.md) | Returns a custom field for estimates by ID ||
    || [crm.quote.userfield.list](./crm-quote-user-field-list.md) | Returns a list of custom fields for estimates by filter ||
    || [crm.quote.userfield.delete](./crm-quote-user-field-delete.md) | Deletes a custom field for estimates ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [onCrmQuoteUserFieldAdd](./events/on-crm-quote-user-field-add.md) | When a custom field is added manually or via the method [crm.quote.userfield.add](./crm-quote-user-field-add.md) ||
    || [onCrmQuoteUserFieldUpdate](./events/on-crm-quote-user-field-update.md) | When a custom field is updated manually or via the method [crm.quote.userfield.update](./crm-quote-user-field-update.md) ||
    || [onCrmQuoteUserFieldDelete](./events/on-crm-quote-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.quote.userfield.delete](./crm-quote-user-field-delete.md) ||
    || [onCrmQuoteUserFieldSetEnumValues](./events/on-crm-quote-user-field-set-enum-values.md) | When list items in an `enumeration` field are changed manually or via the method [crm.quote.userfield.update](./crm-quote-user-field-update.md) ||
    |#

{% endlist %}
