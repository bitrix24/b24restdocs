# About Custom Fields for Requisites

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute methods: any user

This section presents methods for working with custom fields for requisites. Custom fields can only be created for requisites of the following types: [`string`](../../universal/user-defined-fields/crm-userfield-types.md), [`boolean`](../../universal/user-defined-fields/crm-userfield-types.md), [`double`](../../universal/user-defined-fields/crm-userfield-types.md), [`datetime`](../../universal/user-defined-fields/crm-userfield-types.md).

For a description of general methods for working with custom fields, see the section [Custom Fields in CRM](../../universal/user-defined-fields/index.md).

## Fields Describing a Custom Field for Requisites

#|
|| **Name**
`type` | **Description** | **Read** | **Write** | **Required** | **Immutable** | **Multiple** ||
|| **ID**
[`int`](../../../data-types.md) | Identifier of the custom field | Yes | No | No | No | No ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Identifier of the entity to which the custom field belongs. For requisites, this is always `CRM_REQUISITE` | Yes | Yes | Yes | Yes | No ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Symbolic code. For requisites, it always starts with the prefix `UF_CRM_` | Yes | Yes | Yes | Yes | No ||
|| **USER_TYPE_ID**
[`string`](../../../data-types.md) | Data type ([`string`](../../universal/user-defined-fields/crm-userfield-types.md), [`boolean`](../../universal/user-defined-fields/crm-userfield-types.md), [`double`](../../universal/user-defined-fields/crm-userfield-types.md) or [`datetime`](../../universal/user-defined-fields/crm-userfield-types.md)) | Yes | Yes | Yes | Yes | No ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the object in the external information base. 

The purpose of the field may change by the final developer | Yes | Yes | No | No | No ||
|| **SORT**
[`int`](../../../data-types.md) | Sorting | Yes | Yes | No | No | No ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Indicator of multiplicity. Possible values:
- `Y` — yes
- `N` — no 
||
|| **MANDATORY**
[`char`](../../../data-types.md) | Indicator of necessity. Possible values:
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
[`char`](../../../data-types.md) | Show in the list. Possible values:
- `Y` — yes
- `N` — no 
||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Allow user editing. Possible values:
- `Y` — yes
- `N` — no 
||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Are the field values included in the search. Possible values:
- `Y` — yes
- `N` — no 
||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md) | Label in the edit form | Yes | Yes | No | No | No ||
|| **LIST_COLUMN_LABEL**
[`string`](../../../data-types.md) | Header in the list | Yes | Yes | No | No | No ||
|| **LIST_FILTER_LABEL**
[`string`](../../../data-types.md) | Filter label in the list | Yes | Yes | No | No | No ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md) | Error message | Yes | Yes | No | No | No ||
|| **HELP_MESSAGE**
[`string`](../../../data-types.md) | Help | Yes | Yes | No | No | No ||
|| **LIST**
[`uf_enum_element`](../../../data-types.md) | List elements. For detailed information, see the section [{#T}](../../universal/user-defined-fields/crm-userfield-enumeration-fields.md) | Yes | Yes | No | No | Yes ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional settings (dependent on type). For detailed information, see the section [{#T}](../../universal/user-defined-fields/crm-userfield-settings-fields.md) | Yes | Yes | No | No | No ||
|#

## Overview of Methods

#|
|| **Method** | **Description** ||
|| [crm.requisite.userfield.add.md](crm-requisite-userfield-add.md) | Creates a new custom field for a requisite ||
|| [crm.requisite.userfield.update.md](crm-requisite-userfield-update.md) | Modifies an existing custom field for a requisite ||
|| [crm.requisite.userfield.get.md](crm-requisite-userfield-get.md) | Returns a custom field for a requisite by identifier ||
|| [crm.requisite.userfield.list.md](crm-requisite-userfield-list.md) | Returns a list of custom fields for requisites by filter ||
|| [crm.requisite.userfield.delete.md](crm-requisite-userfield-delete.md) | Deletes a custom field for a requisite ||
|#