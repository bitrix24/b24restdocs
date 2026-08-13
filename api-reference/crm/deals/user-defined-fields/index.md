# Custom Fields for Deals: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields store information about a deal in various data formats: string, number, link, address, and others. A field is created once and appears in the detail forms of all deals in Bitrix24.

> Quick navigation: [all methods and events](#all-methods)
>
> User Documentation: [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## Custom Field Names

The field name is assembled by Bitrix24, not by the developer. The `UF_CRM_` prefix is always added to the code you pass in the `FIELD_NAME` parameter of the method [crm.deal.userfield.add](./crm-deal-userfield-add.md): the code `MANAGER_NOTE` becomes the name `UF_CRM_MANAGER_NOTE`. The complete conversion rules are described on the method page.

The resulting name is the key used to read and write the field value in the other deal methods:

- in the response of the methods [crm.deal.get](../crm-deal-get.md) and [crm.deal.list](../crm-deal-list.md), the value arrives in the `UF_CRM_MANAGER_NOTE` field
- in the methods [crm.deal.add](../crm-deal-add.md) and [crm.deal.update](../crm-deal-update.md), the value is passed in `fields` under the same name
- the method [crm.deal.fields](../crm-deal-fields.md) returns custom fields together with system ones

## Types of Custom Fields

The field type is set with the `USER_TYPE_ID` parameter at creation. The list of available types with their identifiers and names is returned by the method [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md).

```json
{
    "result": [
        {
            "ID": "double",
            "title": "Number"
        }
    ]
}
```

The method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md) returns the characteristics that can be set for any custom field — whether it is mandatory or multiple, its sort order, and others.

```json
{
    "result": {
        "MANDATORY": {
            "type": "char",
            "title": "Mandatory"
        }
    }
}
```

## Settings for Custom Fields

Each field type has its own set of additional settings — the `SETTINGS` parameter. The list of settings supported by a specific type is returned by the method [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md).

```json
{
    "result": {
        "DEFAULT_VALUE": {
            "type": "double",
            "title": "Default value"
        },
        "PRECISION": {
            "type": "int",
            "title": "Precision"
        }
    }
}
```

## Errors When Working with Custom Fields

When creating or deleting custom fields, the request may be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal server error. The cause can be found in the server logs at the time of the request.

* In the cloud Bitrix24, submit a request to [technical support](../../../../bitrix-support.md) to get details about the error.
* In the on-premise Bitrix24, request the server error log from the server administrator or hosting administrator, then contact [technical support](../../../../bitrix-support.md) and attach the log for analysis.

### Common Causes of Server Errors

1. You can create 1016 custom fields for deals — this is a limitation of the database architecture. If there are already 1016 fields for deals in Bitrix24, attempting to create a new field will result in the method [crm.deal.userfield.add](./crm-deal-userfield-add.md) returning an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    You can check the number of custom fields for deals using the method [crm.deal.userfield.list](./crm-deal-userfield-list.md).

2. There is a limitation on servers for the execution time of a single request — `max_execution_time`. The standard time is 60 seconds. If the request takes longer, it is interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    The time for [creating](./crm-deal-userfield-add.md) or [deleting](./crm-deal-userfield-delete.md) a custom field for deals depends on the number of deals. When a field is created, it is added to all deal detail forms. When a field is deleted, it is removed from all detail forms. The fewer deals in Bitrix24, the faster fields are created and deleted.

    To check the number of deals, use the method [crm.deal.list](../crm-deal-list.md).

## Overview of Methods and Events {#all-methods}

The events notify about the configuration of the fields themselves, not about changes to their values in deals. How to subscribe to them is described on the page [events for custom deal fields](./events/index.md).

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — creating, modifying, and deleting a field is available to a CRM administrator with the "Allow to change settings" access permission, while any user with the "read" access permission for deals can retrieve a field and a list of fields. Any user can subscribe to the events

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
