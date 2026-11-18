# Custom Lead Fields: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Custom fields store information about a lead in various data formats: string, number, link, address, and others.

{% note warning "" %}

The development of the `crm.lead.userfield.*` methods has been halted. For new development, use the REST 3.0 [crm.lead.field.custom.*](../../rest-v3/fields/index.md) methods. The REST 3.0 methods do not create List `enumeration` and File `file` fields. Use [crm.lead.userfield.add](./crm-lead-userfield-add.md) for these types.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## How to Create a Custom Field

1. Select the field type. The list of available types is returned by the [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) method
2. Review which settings the selected type supports with the [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) method
3. Create the field with the [crm.lead.userfield.add](./crm-lead-userfield-add.md) method
4. Check the result with the [crm.lead.userfield.list](./crm-lead-userfield-list.md) method. The full name of the created field starts with the `UF_CRM_` prefix
5. Fill in the field in leads with the [crm.lead.add](../crm-lead-add.md) and [crm.lead.update](../crm-lead-update.md) methods, passing its full name

The `crm.lead.userfield.*` methods work only with lead fields. To manage fields of different objects with one group of methods, use [userfieldconfig.*](../../universal/userfieldconfig/index.md). The choice between these two groups is described on the [Custom Fields in CRM](../../universal/user-defined-fields/index.md) page.

## Custom Field Settings

The [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md) method describes the characteristics of the field rather than the value stored in it.

The set of settings depends on the field type. Pass the keys retrieved with the [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) method in the `SETTINGS` object of the [crm.lead.userfield.add](./crm-lead-userfield-add.md) and [crm.lead.userfield.update](./crm-lead-userfield-update.md) methods.

## Permissions for Working with Fields

Only a user with the "Edit CRM preferences" permission in CRM can create, modify, and delete custom fields. This is a single role permission for the entire CRM module — it cannot be granted for leads separately.

Any user with read access to leads can read fields with the [crm.lead.userfield.get](./crm-lead-userfield-get.md) and [crm.lead.userfield.list](./crm-lead-userfield-list.md) methods.

## Errors When Working with Custom Fields {#errors}

When creating or deleting custom fields, the request may be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal server error. The cause can be found in the server logs at the time of the request.

* In cloud Bitrix24, submit a request to [technical support](../../../../bitrix-support.md) to get details about the error.
* In on-premise Bitrix24, request the server error log from the server administrator or hosting administrator. Then, contact [technical support](../../../../bitrix-support.md) and attach the log for analysis.

### Common Causes of Server Errors

1. Up to 1016 custom fields can be created for leads—this is a limitation of the database architecture. If there are already 1016 fields for leads in Bitrix24, attempting to create a new field will result in the method [crm.lead.userfield.add](./crm-lead-userfield-add.md) returning an `INTERNAL_SERVER_ERROR` error.

    You can check the number of custom fields for leads using the method [crm.lead.userfield.list](./crm-lead-userfield-list.md).

2. There is a limitation on servers for the execution time of a single request—`max_execution_time`. The standard time is 60 seconds. If the request takes longer, it will be interrupted with an `INTERNAL_SERVER_ERROR` error.

    The time for [creating](./crm-lead-userfield-add.md) or [deleting](./crm-lead-userfield-delete.md) a custom field for leads depends on the number of leads. When a field is created, it is added to all lead detail forms. When a field is deleted, it disappears from all detail forms.

    To check the number of leads, use the method [crm.lead.list](../crm-lead-list.md).

## Overview of Methods and Events {#all-methods}

The events report changes to the field settings and list options. Changing a field value in a lead does not trigger these events. Learn more in the [events overview](./events/index.md).

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
    || [onCrmLeadUserFieldSetEnumValues](./events/on-crm-lead-user-field-set-enum-values.md) | When values are set for a list-type custom field manually, when a list-type field is created via the method [crm.lead.userfield.add](./crm-lead-userfield-add.md), or when the method [crm.lead.userfield.update](./crm-lead-userfield-update.md) is called with the `LIST` parameter ||
    |#

{% endlist %}