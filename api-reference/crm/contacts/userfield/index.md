# Custom Contact Fields: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields store information about a [contact](../index.md) in various data formats: string, number, link, address, and others. The group of methods `crm.contact.userfield.*` creates such fields, changes their settings, returns the list, and deletes them.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## How to Get Started

1. Find out the available field types with the [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) method — the type identifier is passed in the `USER_TYPE_ID` field.
2. Check which settings the selected type supports with the [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) method — their values are passed in the `SETTINGS` object.
3. Create a field with the [crm.contact.userfield.add](./crm-contact-userfield-add.md) method. The required fields of the request are `FIELD_NAME` and `USER_TYPE_ID`.
4. Check the result with the [crm.contact.userfield.list](./crm-contact-userfield-list.md) method: the system adds the `UF_CRM_` prefix to the code you pass, so the resulting field name differs from the one you sent.
5. Write the field value into a contact with the [crm.contact.add](../crm-contact-add.md) and [crm.contact.update](../crm-contact-update.md) methods — pass it together with the system fields in `fields`.
6. Subscribe to the [custom field events](./events/index.md) if your application needs to know about changes in the structure of the fields.

## Types of Custom Fields

Use the method [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) to retrieve the available types of custom fields. The method returns the identifier and name of each type.

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

When creating or deleting custom fields, the request may be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal server error. The reason for the error can be found in the server logs at the time of the request:

* In the cloud Bitrix24, submit a request to [technical support](../../../../bitrix-support.md) to get details about the error.
* In the on-premise Bitrix24, request the server error log from the server administrator or hosting administrator. Then, contact [technical support](../../../../bitrix-support.md) and attach the log for analysis.

### Common Causes of Server Errors

1. You can create 1016 custom fields for contacts — this is a limitation of the database architecture. If there are already 1016 fields for contacts in Bitrix24, attempting to create a new field will result in the method [crm.contact.userfield.add](./crm-contact-userfield-add.md) returning an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    You can check the number of custom fields for contacts using the method [crm.contact.userfield.list](./crm-contact-userfield-list.md).

2. There is a limitation on servers for the execution time of a single request — `max_execution_time`. The standard time is 60 seconds. If the request takes longer, it is interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    The time for [creating](./crm-contact-userfield-add.md) or [deleting](./crm-contact-userfield-delete.md) a contact custom field depends on the number of contacts. When a field is created, it is added to all contact detail forms. When a field is deleted, it is removed from all detail forms. The fewer contacts you have in your Bitrix24, the faster fields are created and deleted.

    To check the number of contacts in Bitrix24, use the method [crm.contact.list](../crm-contact-list.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — creating, modifying, and deleting custom fields is available only to a CRM administrator, while retrieving a field and a list of fields is available to any user with the "read" access permission for contacts. Any user can subscribe to the events

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.contact.userfield.add](./crm-contact-userfield-add.md) | Creates a new custom field for contacts ||
    || [crm.contact.userfield.update](./crm-contact-userfield-update.md) | Updates an existing custom field for contacts ||
    || [crm.contact.userfield.get](./crm-contact-userfield-get.md) | Returns a custom field for contacts by ID ||
    || [crm.contact.userfield.list](./crm-contact-userfield-list.md) | Returns a list of custom fields for contacts by filter ||
    || [crm.contact.userfield.delete](./crm-contact-userfield-delete.md) | Deletes a custom field for contacts ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmContactUserFieldAdd](./events/on-crm-contact-user-field-add.md) | When a custom field is added ||
    || [onCrmContactUserFieldUpdate](./events/on-crm-contact-user-field-update.md) | When a custom field is updated ||
    || [onCrmContactUserFieldDelete](./events/on-crm-contact-user-field-delete.md) | When a custom field is deleted ||
    || [onCrmContactUserFieldSetEnumValues](./events/on-crm-contact-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed ||
    |#

{% endlist %}