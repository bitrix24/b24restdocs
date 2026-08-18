# Custom Fields: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In Bitrix24, you can configure custom fields for employees. This allows you to supplement the standard information about users — for example, add a list of skills or a hire date to the profile.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to add custom fields to an employee profile](https://helpdesk.bitrix24.com/open/25556660/)

## How to Work with Custom Fields

1. Create a field using the [user.userfield.add](./user-userfield-add.md) method. In the `FIELD_NAME` parameter, pass the field code — Bitrix24 supplements it with the `UF_USR_` prefix, for example, `UF_USR_SKILLS`.
2. Check the list of created fields and their codes using the [user.userfield.list](./user-userfield-list.md) method.
3. Write the field value to the employee profile using the [user.update](../../user/user-update.md) method — pass the field code together with the user `ID`.
4. Read the values using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods. To retrieve all custom fields, pass the `UF_*` mask in the `select` parameter.
5. Change the settings or the name of the field using the [user.userfield.update](./user-userfield-update.md) method, and delete a field you no longer need using the [user.userfield.delete](./user-userfield-delete.md) method.

The methods for working with the employee profile are described in the [Users](../index.md) section.

{% note warning "" %}

The `user.userfield.*` methods work in the `user.userfield` scope and can be executed only by an administrator. An application with the limited `user_brief` or `user_basic` scopes also needs the `user.userfield` scope to read the values of custom fields.

{% endnote %}

## Types of Custom Fields

For each custom field, a type is specified: String, List, Number, CRM element binding, and others. A complete list of field types is described in the `fields` parameter of the [user.userfield.add](./user-userfield-add.md) method.

{% note warning "" %}

The type is set when creating the custom field and cannot be changed.

{% endnote %}

## Linking Custom Fields with Other Objects

**Currencies.** In the Money type field, you specify a number and a currency. You can work with currencies through the [crm.currency.*](../../crm/currency/index.md) methods.

**User.** In a custom field, you can set a binding to an employee. You can obtain the user ID using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods.

**Information Blocks.** Custom fields can be linked to [products](../../catalog/product/index.md), [workflows](../../bizproc/index.md), and [lists](../../lists/index.md) through the Info block element binding and Info block section binding types.

**Files.** The File type field allows you to upload and store files. Files do not receive IDs in the Drive module, so Drive methods are not applicable. You can only work with files through the Bitrix24 interface.

**CRM.** Custom fields can be linked to CRM objects using two types.

- CRM element binding — for linking with [leads](../../crm/leads/index.md), [deals](../../crm/deals/index.md), [contacts](../../crm/contacts/index.md), and [companies](../../crm/companies/index.md).

- CRM directory binding — for selection from the list of [CRM directories](../../crm/status/index.md).

## Errors When Working with Custom Fields

When creating or deleting custom fields, the request may be interrupted with an [INTERNAL_SERVER_ERROR](../../../error-codes.md). This is an internal server error. The cause of the error can be found in the server logs at the time of the request.

- Cloud Bitrix24 — contact [technical support](../../../bitrix-support.md) to get details about the error.

- On-premise Bitrix24 — request the server error log from the server or hosting administrator. Then, contact [technical support](../../../bitrix-support.md) and attach the log for analysis.

### Common Causes of Server Errors

1. You can create 1016 custom fields for users — this is a database architecture limitation. If the limit is reached, attempting to create a new field will return an [INTERNAL_SERVER_ERROR](../../../error-codes.md) from the [user.userfield.add](./user-userfield-add.md) method.

   You can check the number of custom fields using the [user.userfield.list](./user-userfield-list.md) method.

2. There is a limitation on servers for the execution time of a single request — `max_execution_time`. The default value is 60 seconds. If the request takes longer, it is interrupted with an [INTERNAL_SERVER_ERROR](../../../error-codes.md).

   The time for [creating](./user-userfield-add.md) or [deleting](./user-userfield-delete.md) a custom field depends on the number of users. When a field is created, it is added to all employee profiles. When a field is deleted, it is removed from all profiles. The fewer users in your Bitrix24, the faster the fields will be created and deleted.

   You can check the number of users in Bitrix24 using the [user.get](../../user/user-get.md) method.

## Overview of Methods {#all-methods}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can perform the method: administrator

#|
|| **Method** | **Description** ||
|| [user.userfield.add](user-userfield-add.md) | Adds a custom field ||
|| [user.userfield.update](user-userfield-update.md) | Updates a custom field ||
|| [user.userfield.list](user-userfield-list.md) | Retrieves a list of custom fields ||
|| [user.userfield.delete](user-userfield-delete.md) | Deletes a custom field ||
|#
