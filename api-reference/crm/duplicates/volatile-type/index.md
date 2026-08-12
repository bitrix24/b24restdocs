# Duplicate Search Settings for Any Fields: Overview of Methods

By default, Bitrix24 searches for duplicates using a fixed set of fields: Full Name, company name, phone, email address, and requisites. The methods `crm.duplicate.volatileType.*` extend this set — any standard or custom field of a lead, contact, or company can be added to the search.

The added field appears in the duplicate search settings in the Bitrix24 interface for all employees. These settings do not affect the search performed by [crm.duplicate.findbycomm](../crm-duplicate-find-by-comm.md): that method works only with phone numbers and email addresses. Duplicate handling as a whole is described in the section [Finding and Processing Duplicates in CRM](../index.md).

For example, you can add a company Tax ID to the search — then Bitrix24 shows two companies with the same Tax ID as duplicates.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Searching and Handling Duplicates in Bitrix24](https://helpdesk.bitrix24.com/open/25404806/)

## How to Configure Search by Additional Fields

1. Retrieve the list of available fields using the method [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md) — the response contains `entityTypeId` and `fieldCode` pairs.
2. Check which fields are already connected using the method [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md).
3. Connect the required field using the method [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md), passing the `entityTypeId` and `fieldCode` from the first step.
4. Disconnect a field using the method [crm.duplicate.volatileType.unregister](./crm-duplicate-volatile-type-unregister.md), passing the record `id` from [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md).

## Identifiers and Field Codes

**entityTypeId.** Specifies the CRM object type. Duplicates are searched for across three objects only:

#|
|| **CRM Object Type** | **entityTypeId** ||
|| Lead | `1` ||
|| Contact | `3` ||
|| Company | `4` ||
|#

**fieldCode.** The symbolic code of the field: `TITLE` for the name, `ADDRESS` for the address, `UF_CRM_1750854801` for a custom field. For requisite fields, the code is written with dots, for example `RQ.EN.NAME`. Codes are not constructed manually — take them from the response of [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md) for the required `entityTypeId`. If you pass a code that is not in the list of available ones, [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md) returns the error `FIELD_NOT_FOUND`.

**id.** The identifier of the record about a connected field. It is returned by [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md) and [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md), and it is the only value suitable for disconnecting a field.

## Important Considerations

- No more than seven fields in total can be connected for leads, contacts, and companies. When you attempt to connect the eighth one, [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md) returns the error `MAX_TYPES_COUNT_EXCEEDED`.
- Calling [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md) again for a field that is already connected does not create a new record — the method returns the `id` of the existing one.
- After a field is connected, the duplicate index is recalculated by a background agent, so new matches do not appear in the interface right away.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: Bitrix24 administrator or CRM administrator

#|
|| **Method** | **Description** ||
|| [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md) | Returns a list of standard and custom fields for duplicate searches ||
|| [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md) | Returns a list of additional fields already connected to duplicate searches ||
|| [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md) | Adds a field to the duplicate search settings ||
|| [crm.duplicate.volatileType.unregister](./crm-duplicate-volatile-type-unregister.md) | Removes a field from the duplicate search settings ||
|#
