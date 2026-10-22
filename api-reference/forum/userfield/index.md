# Forum Message Custom Fields: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Custom fields add your own data to forum messages: an external code, a priority, a category, and other values. The `forum.message.field.custom.*` methods create, update, retrieve, and delete such fields. The `forum.message.field.custom.enum.*` methods manage the value options of "List" fields.

> Quick navigation: [all methods](#all-methods)

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

## Getting Started

1. Retrieve the current custom fields with [forum.message.field.custom.list](./forum-message-field-custom-list.md). Bitrix24 already has system fields, for example `UF_FORUM_MESSAGE_DOC` for message files
2. Create a field with [forum.message.field.custom.add](./forum-message-field-custom-add.md). Save the `id` from the response — you need it to update or delete the field and to work with list values
3. For an `enumeration` field, add value options with [forum.message.field.custom.enum.add](./forum-message-field-custom-enum-add.md), passing the field `id` in `fieldId`
4. Check the value options with [forum.message.field.custom.enum.list](./forum-message-field-custom-enum-list.md)

## Identifiers

#|
|| **Identifier** | **What it means** | **Where to get it** ||
|| Field `id` | Numeric identifier of the custom field, for example `6008093` | The response of [forum.message.field.custom.add](./forum-message-field-custom-add.md) or [forum.message.field.custom.list](./forum-message-field-custom-list.md) ||
|| `name` | Field code with the `UF_` prefix, for example `UF_PRIORITY`. Set on creation and cannot be changed | The response of [forum.message.field.custom.list](./forum-message-field-custom-list.md) ||
|| `fieldId` | Identifier of the "List" field the value belongs to. Matches the field `id` | The response of [forum.message.field.custom.list](./forum-message-field-custom-list.md) ||
|| Value `id` | Identifier of a list value option, for example `4079`. The numbering is not related to field `id` values | The response of [forum.message.field.custom.enum.list](./forum-message-field-custom-enum-list.md) ||
|#

The methods substitute the `FORUM_MESSAGE` object code the fields belong to themselves. You do not need to pass `entityId`: a passed value is ignored.

## Limitations and Features {#restrictions}

- The field labels `editFormLabel`, `listColumnLabel`, `listFilterLabel`, `errorMessage`, and `helpMessage` are passed as an object by language, for example `{"de":"Priorität","en":"Priority"}`. A string instead of an object is not saved. In the response, a label is returned as a string in the response language
- If you pass at least one label in [forum.message.field.custom.update](./forum-message-field-custom-update.md), all labels that are not passed are cleared. When changing a label, pass all labels of the field
- The field type settings in `settings` are not saved on creation or update: the field gets the default settings of its type
- The [forum.message.field.custom.list](./forum-message-field-custom-list.md) method always returns all custom fields of messages. Filtering and pagination are not applied
- The [forum.message.field.custom.enum.list](./forum-message-field-custom-enum-list.md) method accepts the filter only as a single condition `["fieldId", 6008093]`, without the outer array. Values are always returned in ascending `sort` order, and pagination is not applied
- An empty `value` in [forum.message.field.custom.enum.update](./forum-message-field-custom-enum-update.md) deletes the list option, although the method returns a successful response
- The `isDefault` flag of a list option is not automatically removed from other options

## Overview of Methods {#all-methods}

> Scope: [`forum`](../../scopes/permissions.md)
>
> Who can execute the methods: any user

### Custom Fields

#|
|| **Method** | **Description** ||
|| [forum.message.field.custom.add](./forum-message-field-custom-add.md) | Adds a custom field to forum messages ||
|| [forum.message.field.custom.update](./forum-message-field-custom-update.md) | Updates a custom field ||
|| [forum.message.field.custom.get](./forum-message-field-custom-get.md) | Returns a custom field by its identifier ||
|| [forum.message.field.custom.list](./forum-message-field-custom-list.md) | Returns the list of custom fields of forum messages ||
|| [forum.message.field.custom.delete](./forum-message-field-custom-delete.md) | Deletes a custom field ||
|#

### Values of "List" Fields

#|
|| **Method** | **Description** ||
|| [forum.message.field.custom.enum.add](./forum-message-field-custom-enum-add.md) | Adds a value to a "List" field ||
|| [forum.message.field.custom.enum.update](./forum-message-field-custom-enum-update.md) | Updates a list value ||
|| [forum.message.field.custom.enum.get](./forum-message-field-custom-enum-get.md) | Returns a list value by its identifier ||
|| [forum.message.field.custom.enum.list](./forum-message-field-custom-enum-list.md) | Returns the values of a "List" field ||
|| [forum.message.field.custom.enum.delete](./forum-message-field-custom-enum-delete.md) | Deletes a list value ||
|#

## Continue Learning

- [{#T}](./forum-message-field-custom-add.md)
- [{#T}](./forum-message-field-custom-enum-add.md)
- [{#T}](../message/index.md)
- [{#T}](../index.md)
