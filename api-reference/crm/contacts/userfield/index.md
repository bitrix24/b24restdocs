# Custom Contact Fields: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Custom fields store contact information in various data formats: string, number, link, address, and others. A field is created once and appears in all Bitrix24 contact detail forms.

{% note warning "" %}

The development of the `crm.contact.userfield.*` methods has been halted. For new development, use the REST 3.0 [crm.contact.field.custom.*](../../rest-v3/fields/index.md) methods. The REST 3.0 methods do not create List `enumeration` and File `file` fields. Use [crm.contact.userfield.add](./crm-contact-userfield-add.md) for these types.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## Custom Field Names

Bitrix24, not the developer, generates the field name. If necessary, the `UF_CRM_` prefix is added to the code passed in the `FIELD_NAME` parameter of [crm.contact.userfield.add](./crm-contact-userfield-add.md): the code `MANAGER_NOTE` becomes the name `UF_CRM_MANAGER_NOTE`. The method page describes the complete conversion rules.

The resulting name is the key used to write and retrieve the field value in contact methods, such as [crm.contact.add](../crm-contact-add.md) and [crm.contact.list](../crm-contact-list.md). The [crm.contact.fields](../crm-contact-fields.md) method returns custom fields together with system fields.

## How to Get Started

1. Select the field type. The [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) method returns the list of types with identifiers for the `USER_TYPE_ID` parameter, for example, `double` for a number. The [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md) method describes the common field properties: whether it is required or multiple, and its sort order.
2. Retrieve the type settings for the `SETTINGS` parameter with [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md), for example, `PRECISION` for number precision. The `crm.contact.userfield.add` method does not accept all type settings. The supported settings are listed on the [method page](./crm-contact-userfield-add.md#settings).
3. Create a field, for example, a `string` field with the code `MANAGER_NOTE`, using [crm.contact.userfield.add](./crm-contact-userfield-add.md). The method returns the field identifier.
4. Retrieve the field name with the `UF_CRM_` prefix by passing the identifier returned by `crm.contact.userfield.add` to [crm.contact.userfield.get](./crm-contact-userfield-get.md), or find it in the [crm.contact.userfield.list](./crm-contact-userfield-list.md) response.
5. Write the value to a contact with [crm.contact.update](../crm-contact-update.md): `"fields": {"UF_CRM_MANAGER_NOTE": "Call back on Friday"}`. For an `enumeration` field, pass the item `ID` from the `LIST` array.
6. Retrieve the value with [crm.contact.get](../crm-contact-get.md). It will be returned in `UF_CRM_MANAGER_NOTE`.

The `crm.contact.userfield.*` methods work only with contact fields. To manage fields of different objects with one group of methods, use [userfieldconfig.*](../../universal/userfieldconfig/index.md). The choice between these two groups is described on the [Custom Fields in CRM](../../universal/user-defined-fields/index.md) page.

The custom field object structure is described in the [`userfield`](./crm-contact-userfield-get.md#userfield) section.

## Errors When Working with Custom Fields {#errors}

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

Events report changes to field settings, not to their values in contacts. For subscription instructions, see [Custom Contact Field Events](./events/index.md).

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method. A user with the "Edit CRM preferences" permission in CRM can create, modify, and delete fields. Any user with read access to contacts can retrieve a field and a list of fields. Any user can subscribe to events. Administrator permissions are required for offline events

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.contact.userfield.add](./crm-contact-userfield-add.md) | Creates a new custom field for contacts ||
    || [crm.contact.userfield.update](./crm-contact-userfield-update.md) | Updates an existing custom field for contacts ||
    || [crm.contact.userfield.get](./crm-contact-userfield-get.md) | Returns a custom field for contacts by ID ||
    || [crm.contact.userfield.list](./crm-contact-userfield-list.md) | Returns a list of custom fields for contacts ||
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