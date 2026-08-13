# Custom Company Fields: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields store information about a company in various data formats: string, number, link, address, and others. The group of methods `crm.company.userfield.*` creates, modifies, retrieves, and deletes such fields, and the [events of the subsection](./events/index.md) notify the application about changes in them.

General information about companies and the other groups of methods is in the section [Companies in CRM](../index.md).

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## Types of Custom Fields

Use the method [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) to retrieve the available types of custom fields. The method will return the identifier and the name of each type. The identifier is passed in the `USER_TYPE_ID` field when creating a field.

```json
{
    "ID": "double",
    "title": "Number"
}
```

Use the method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md) to get a list of characteristics of custom fields. The method will return the codes of the characteristics along with their type and name.

```json
"MANDATORY": {
    "type": "char",
    "title": "Required"
}
```

## Custom Field Settings

Use the method [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) to obtain a list of available settings. The method will return the supported settings for the requested field type.

```json
"DEFAULT_VALUE": {
    "type": "double",
    "title": "Default value"
},
"PRECISION": {
    "type": "int",
    "title": "Precision"
}
```

## Errors When Working with Custom Fields

When creating or deleting custom fields, the request may be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal server error. The cause of the error can be found in the server logs at the time of the request:

- In cloud Bitrix24, submit a request to [technical support](../../../../bitrix-support.md) to obtain error details.
- In on-premise Bitrix24, request the server error log from the server administrator or hosting administrator. Then, contact [technical support](../../../../bitrix-support.md) and attach the log for analysis.

### Common Causes of Server Errors

1. Up to 1016 custom fields can be created for companies—this is a limitation of the database architecture. If there are already 1016 fields for companies in Bitrix24, attempting to create a new field will result in an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md) when using the method [crm.company.userfield.add](./crm-company-userfield-add.md).

    You can check the number of custom fields for companies using the method [crm.company.userfield.list](./crm-company-userfield-list.md).

2. There is a limitation on servers for the execution time of a single request—`max_execution_time`. The standard time is 60 seconds. If the request takes longer, it will be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

   The time to create or delete a custom field for a company depends on the number of companies. When a field is created, it is added to all company detail forms. When a field is deleted, it is removed from all detail forms. The fewer companies in your Bitrix24, the faster fields are created and deleted.

   To check the number of companies in Bitrix24, use the method [crm.company.list](../crm-company-list.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.company.userfield.add](./crm-company-userfield-add.md) | Creates a new custom field for companies ||
    || [crm.company.userfield.update](./crm-company-userfield-update.md) | Updates an existing custom field for companies ||
    || [crm.company.userfield.get](./crm-company-userfield-get.md) | Returns a custom field for companies by ID ||
    || [crm.company.userfield.list](./crm-company-userfield-list.md) | Returns a list of custom fields for companies by filter ||
    || [crm.company.userfield.delete](./crm-company-userfield-delete.md) | Deletes a custom field for companies ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmCompanyUserFieldAdd](./events/on-crm-company-user-field-add.md) | When a custom field is added manually or via the method [crm.company.userfield.add](./crm-company-userfield-add.md) ||
    || [onCrmCompanyUserFieldUpdate](./events/on-crm-company-user-field-update.md) | When a custom field is modified manually or via the method [crm.company.userfield.update](./crm-company-userfield-update.md) ||
    || [onCrmCompanyUserFieldDelete](./events/on-crm-company-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.company.userfield.delete](./crm-company-userfield-delete.md) ||
    || [onCrmCompanyUserFieldSetEnumValues](./events/on-crm-company-user-field-set-enum-values.md) | When the set of values for a custom field of list type is changed manually or via the methods [crm.company.userfield.add](./crm-company-userfield-add.md) and [crm.company.userfield.update](./crm-company-userfield-update.md) ||
    |#

{% endlist %}
