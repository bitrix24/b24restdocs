# Custom Fields for Estimates: Overview of Methods and Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields add data to the estimate detail form for which there are no system fields: a string, a number, a link, an address, a reference to another CRM object. The methods for working with the estimates themselves are collected in the section [Estimates](../index.md).

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## Getting Started

1. Choose the field type — it determines the format of the stored data and the set of available settings. The full list of types is returned by the method [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md)
2. Find out which settings the chosen type supports using the method [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md), and the list of general field characteristics using the method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md)
3. Create the field using the method [crm.quote.userfield.add](./crm-quote-user-field-add.md). The response contains the field identifier, and the full name is returned by the method [crm.quote.userfield.get](./crm-quote-user-field-get.md)
4. Write and read the field value by its full name in the methods [crm.quote.add](../crm-quote-add.md), [crm.quote.update](../crm-quote-update.md), and [crm.quote.get](../crm-quote-get.md)

## Custom Field Name

The field code is passed in the required parameter `FIELD_NAME` of the method [crm.quote.userfield.add](./crm-quote-user-field-add.md), and the system builds the full name itself: the prefix `UF_CRM_` is added to the code. For `FIELD_NAME = MANAGER_NOTE`, the full field name will be `UF_CRM_MANAGER_NOTE`. For fields created in the Bitrix24 interface, the code is substituted automatically, and their names look like `UF_CRM_QUOTE_1768404002`.

It is the full name that is used further on:

- in the `fields` parameter of the methods [crm.quote.add](../crm-quote-add.md) and [crm.quote.update](../crm-quote-update.md) — to write the value
- in the `select` parameter of the method [crm.quote.list](../crm-quote-list.md) — to return the value in the selection. The `UF_*` mask selects all custom fields at once
- in the `FIELD_NAME` field of custom field [events](./events/index.md) — to determine which field has changed

## Types of Custom Fields

The type is set in the `USER_TYPE_ID` field when the field is created and cannot be changed afterwards.

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
|| `enumeration` | A value from a list that is defined in the `LIST` field ||
|| `employee` | A reference to a Bitrix24 user ||
|| `crm` | A reference to a CRM entity: a lead, deal, contact, or company ||
|| `crm_status` | A reference to a CRM reference guide item ||
|| `iblock_element` | A reference to an information block element ||
|| `iblock_section` | A reference to an information block section ||
|#

The current list of types is returned by the method [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md), and the set of settings for a specific type — by the method [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md).

## Errors When Working with Custom Fields

When creating or deleting custom fields, the request may be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal server error, and its cause is visible only in the server logs at the time of the request:

- in cloud Bitrix24, submit a request to [technical support](../../../../bitrix-support.md) to get details about the error
- in on-premise Bitrix24, request the server error log from the server or hosting administrator, and then contact [technical support](../../../../bitrix-support.md) and attach the log for analysis

### Common Causes of Server Errors

1. You can create 1016 custom fields for estimates — this is a limitation of the database architecture. If there are already 1016 fields for estimates in Bitrix24, attempting to create a new field will result in the method [crm.quote.userfield.add](./crm-quote-user-field-add.md) returning an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md)

    You can check the number of custom fields for estimates using the method [crm.quote.userfield.list](./crm-quote-user-field-list.md)

2. There is a limitation on servers for the execution time of a single request — `max_execution_time`, with a standard value of 60 seconds. If the request takes longer, it is interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md)

    The time for [creating](./crm-quote-user-field-add.md) or [deleting](./crm-quote-user-field-delete.md) a custom field depends on the number of estimates: when a field is created, it is added to all estimate detail forms, and when it is deleted, it is removed from all detail forms. The fewer estimates you have in Bitrix24, the faster fields are created and deleted

    To find out the number of estimates, use the method [crm.quote.list](../crm-quote-list.md)

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — only a CRM administrator can create, update, and delete fields, while a user with permission to read estimates can read them

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
    || [onCrmQuoteUserFieldSetEnumValues](./events/on-crm-quote-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed manually or via the method [crm.quote.userfield.update](./crm-quote-user-field-update.md) ||
    |#

{% endlist %}
