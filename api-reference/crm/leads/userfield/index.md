# Custom Lead Fields: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields store information about a lead in various data formats: string, number, link, address, and others.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## How to Create a Custom Field

1. Select the field type. The list of available types is returned by the [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) method.
2. Review which settings the selected type supports with the [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) method.
3. Create the field with the [crm.lead.userfield.add](./crm-lead-userfield-add.md) method. The required parameters are `FIELD_NAME` and `USER_TYPE_ID`.
4. Check the result with the [crm.lead.userfield.list](./crm-lead-userfield-list.md) method. The full name of the created field starts with the `UF_CRM_` prefix.
5. Fill in the field in leads with the [crm.lead.add](../crm-lead-add.md) and [crm.lead.update](../crm-lead-update.md) methods, passing its full name.

## Types of Custom Fields

The [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) method returns the available field types. Pass the `ID` value to the `USER_TYPE_ID` parameter when creating a field.

```json
[
    {
        "ID": "string",
        "title": "String"
    },
    {
        "ID": "double",
        "title": "Number"
    }
]
```

The [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md) method returns the list of characteristics of the custom field itself — those that describe the field rather than the value stored in it.

```json
{
    "FIELD_NAME": {
        "type": "string",
        "title": "Code",
        "isImmutable": true
    },
    "MANDATORY": {
        "type": "char",
        "title": "Required"
    }
}
```

## Custom Field Settings

The set of settings depends on the field type. The [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) method returns the settings supported by the requested type. For example, for the `double` type:

```json
{
    "DEFAULT_VALUE": {
        "type": "double",
        "title": "Default value"
    },
    "PRECISION": {
        "type": "int",
        "title": "Precision"
    }
}
```

Pass the retrieved keys in the `SETTINGS` object of the [crm.lead.userfield.add](./crm-lead-userfield-add.md) and [crm.lead.userfield.update](./crm-lead-userfield-update.md) methods.

## Permissions for Working with Fields

Only a user with the "Allow to modify settings" permission in CRM can create, modify, and delete custom fields. This is a single role permission for the entire CRM module — it cannot be granted for leads separately.

Any user with read access to leads can read fields with the [crm.lead.userfield.get](./crm-lead-userfield-get.md) and [crm.lead.userfield.list](./crm-lead-userfield-list.md) methods.

## Errors When Working with Custom Fields

When creating or deleting custom fields, the request may be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal server error. The cause of the error can be found in the server logs at the time of the request:

* In cloud Bitrix24, submit a request to [technical support](../../../../bitrix-support.md) to get details about the error.
* In on-premise Bitrix24, request the server error log from the server administrator or hosting administrator. Then, contact [technical support](../../../../bitrix-support.md) and attach the log for analysis.

### Common Causes of Server Errors

1. Up to 1016 custom fields can be created for leads—this is a limitation of the database architecture. If there are already 1016 fields for leads in Bitrix24, attempting to create a new field will result in the method [crm.lead.userfield.add](./crm-lead-userfield-add.md) returning an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    You can check the number of custom fields for leads using the method [crm.lead.userfield.list](./crm-lead-userfield-list.md).

2. There is a limitation on servers for the execution time of a single request—`max_execution_time`. The standard time is 60 seconds. If the request takes longer, it will be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    The time for [creating](./crm-lead-userfield-add.md) or [deleting](./crm-lead-userfield-delete.md) a custom field for leads depends on the number of leads. When a field is created, it is added to all lead detail forms. When a field is deleted, it is removed from all detail forms. The fewer leads in your Bitrix24, the faster fields are created and deleted.

    To check the number of leads in Bitrix24, use the method [crm.lead.list](../crm-lead-list.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform the method: depending on the method

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [crm.lead.userfield.add](./crm-lead-userfield-add.md) | Creates a new custom field for leads ||
    || [crm.lead.userfield.update](./crm-lead-userfield-update.md) | Updates an existing custom field for leads ||
    || [crm.lead.userfield.get](./crm-lead-userfield-get.md) | Returns a custom field for leads by ID ||
    || [crm.lead.userfield.list](./crm-lead-userfield-list.md) | Returns a list of custom fields for leads by filter ||
    || [crm.lead.userfield.delete](./crm-lead-userfield-delete.md) | Deletes a custom field for leads ||
    |#

- Events 

    #| 
    || **Event** | **Triggered** ||
    || [onCrmLeadUserFieldAdd](./events/on-crm-lead-user-field-add.md) | When a custom field is added manually or via the method [crm.lead.userfield.add](./crm-lead-userfield-add.md) ||
    || [onCrmLeadUserFieldUpdate](./events/on-crm-lead-user-field-update.md) | When a custom field is updated manually or via the method [crm.lead.userfield.update](./crm-lead-userfield-update.md) ||
    || [onCrmLeadUserFieldDelete](./events/on-crm-lead-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.lead.userfield.delete](./crm-lead-userfield-delete.md) ||
    || [onCrmLeadUserFieldSetEnumValues](./events/on-crm-lead-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed manually or via the method [crm.lead.userfield.update](./crm-lead-userfield-update.md) ||
    |#

{% endlist %}