# CRM Company Details Custom Fields: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Company details custom fields supplement the standard set of company details fields with integration-specific or business-specific data.

Custom fields for company details can only be created for the following types: [`string`](../../universal/user-defined-fields/crm-userfield-types.md), [`boolean`](../../universal/user-defined-fields/crm-userfield-types.md), [`double`](../../universal/user-defined-fields/crm-userfield-types.md), [`datetime`](../../universal/user-defined-fields/crm-userfield-types.md).

General rules for working with custom fields are described on the [Custom Fields in CRM](../../universal/user-defined-fields/index.md) page.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## Getting Started

1. Specify the custom field object `ENTITY_ID` with the value `CRM_REQUISITE`
2. Select a data type `USER_TYPE_ID`: `string`, `boolean`, `double`, or `datetime`
3. Set a symbolic code `FIELD_NAME` with the prefix `UF_CRM_`
4. Create the field using the [crm.requisite.userfield.add](./crm-requisite-userfield-add.md) method
5. Retrieve a list of custom fields using the [crm.requisite.userfield.list](./crm-requisite-userfield-list.md) method
6. Update or delete a field using the [crm.requisite.userfield.update](./crm-requisite-userfield-update.md) and [crm.requisite.userfield.delete](./crm-requisite-userfield-delete.md) methods

## Custom Field Identifiers

- `ID` — the custom field identifier. It is returned by the [crm.requisite.userfield.add](./crm-requisite-userfield-add.md), [crm.requisite.userfield.get](./crm-requisite-userfield-get.md), and [crm.requisite.userfield.list](./crm-requisite-userfield-list.md) methods
- `ENTITY_ID` — the identifier of the object to which the custom field belongs. For company details, always pass `CRM_REQUISITE`
- `FIELD_NAME` — the symbolic code of the custom field. For company details, the code always begins with the prefix `UF_CRM_`
- `USER_TYPE_ID` — the data type of the custom field. Available values: `string`, `boolean`, `double`, `datetime`

## Fields Describing a Company Details Custom Field

Required fields are marked with `*`.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`int`](../../../data-types.md) | Identifier of the custom field ||
|| **ENTITY_ID***
[`string`](../../../data-types.md) | Identifier of the object to which the custom field belongs. For requisites, this is always `CRM_REQUISITE` ||
|| **FIELD_NAME***
[`string`](../../../data-types.md) | Symbolic code. For requisites, it always starts with the prefix `UF_CRM_` ||
|| **USER_TYPE_ID***
[`string`](../../../data-types.md) | Data type ([`string`](../../universal/user-defined-fields/crm-userfield-types.md), [`boolean`](../../universal/user-defined-fields/crm-userfield-types.md), [`double`](../../universal/user-defined-fields/crm-userfield-types.md), or [`datetime`](../../universal/user-defined-fields/crm-userfield-types.md)) ||
|| **XML_ID**
[`string`](../../../data-types.md) | Foreign key. Used for exchange operations. Identifier of the external information base object.

The purpose of the field may be changed by the end developer ||
|| **SORT**
[`int`](../../../data-types.md) | Sorting ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Indicator of multiplicity. Possible values:
- `Y` — yes
- `N` — no
||
|| **MANDATORY**
[`char`](../../../data-types.md) | Indicator of mandatory status. Possible values:
- `Y` — yes
- `N` — no
||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Show in the list filter. Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring
||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Whether to show in the list. Possible values:
- `Y` — yes
- `N` — no
||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Allow user editing? Possible values:
- `Y` — yes
- `N` — no
||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Whether the field values participate in search. Possible values:
- `Y` — yes
- `N` — no
||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md) | Label in the edit form ||
|| **LIST_COLUMN_LABEL**
[`string`](../../../data-types.md) | Header in the list ||
|| **LIST_FILTER_LABEL**
[`string`](../../../data-types.md) | Filter label in the list ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md) | Error message ||
|| **HELP_MESSAGE**
[`string`](../../../data-types.md) | Help ||
|| **LIST**
[`uf_enum_element`](../../../data-types.md) | List items. A detailed description of the item fields is available on the [{#T}](../../universal/user-defined-fields/crm-userfield-enumeration-fields.md) page ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional settings that depend on the field type. A detailed description of the settings is available on the [{#T}](../../universal/user-defined-fields/crm-userfield-settings-fields.md) page ||
|#

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [crm.requisite.userfield.add](./crm-requisite-userfield-add.md) | Creates a new custom field for a requisite ||
|| [crm.requisite.userfield.update](./crm-requisite-userfield-update.md) | Modifies an existing custom field for a requisite ||
|| [crm.requisite.userfield.get](./crm-requisite-userfield-get.md) | Returns a custom field for a requisite by identifier ||
|| [crm.requisite.userfield.list](./crm-requisite-userfield-list.md) | Returns a list of custom fields for requisites by filter ||
|| [crm.requisite.userfield.delete](./crm-requisite-userfield-delete.md) | Deletes a custom field for a requisite ||
|#
