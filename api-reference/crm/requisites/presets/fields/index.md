# Customizable Company Details Template Fields: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields are fields that can be added or removed from the template.

In the company details editing form, besides the system fields present in any company details record (for example, `Name` or `Comment`), there are other fields (such as Tax ID and `KPP`) that are customizable. The methods in this section are designed to manage the list of such fields for a specific template.

Custom fields can be divided into two categories: pre-installed and user-defined.

The set of pre-installed fields is fixed for each supported country. Any such field can be added to a template related to the corresponding country if it is not already present in the template. Pre-installed fields have the prefix `RQ_`.

User-defined fields are added either by the user or through the corresponding [REST methods](../../user-fields/index.md). A created user-defined field can be added to a template. User-defined fields can be added to templates of any supported country. User-defined fields have the prefix `UF_`. If a user-defined field is used in templates for different countries, it is essential to ensure that it has the same meaning and name in the interface for them.

The **FIELD_NAME** field is essentially a reference to an existing field, either pre-installed or user-defined.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [How to Create and Configure Company Details Templates in CRM](https://helpdesk.bitrix24.com/open/25770635/)

## Getting Started

1. Retrieve the company details template identifier using the [crm.requisite.preset.list](../crm-requisite-preset-list.md) method
2. Check the fields available to add using the [crm.requisite.preset.field.availabletoadd](./crm-requisite-preset-field-available-to-add.md) method
3. Add a field to the template using the [crm.requisite.preset.field.add](./crm-requisite-preset-field-add.md) method
4. Change the name or sorting using the [crm.requisite.preset.field.update](./crm-requisite-preset-field-update.md) method
5. Retrieve the list of template fields using the [crm.requisite.preset.field.list](./crm-requisite-preset-field-list.md) method
6. Remove a field from the template using the [crm.requisite.preset.field.delete](./crm-requisite-preset-field-delete.md) method

## Template Field Identifiers

- `preset.ID` — the company details template identifier. This can be retrieved using the [crm.requisite.preset.list](../crm-requisite-preset-list.md) method
- `ID` — the field identifier within a specific template. This is returned by the [crm.requisite.preset.field.add](./crm-requisite-preset-field-add.md), [crm.requisite.preset.field.get](./crm-requisite-preset-field-get.md), and [crm.requisite.preset.field.list](./crm-requisite-preset-field-list.md) methods
- `FIELD_NAME` — the code of the pre-installed or user-defined field being added to the template. Available values are returned by the [crm.requisite.preset.field.availabletoadd](./crm-requisite-preset-field-available-to-add.md) method

## Fields Describing the Custom Field in the Requisite Template

Required fields are marked with `*`.

#|
||  **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Field identifier. Created automatically and unique within the template ||
|| **FIELD_NAME***
[`string`](../../../../data-types.md) | Field name ||
|| **FIELD_TITLE**
[`string`](../../../../data-types.md) | An alternative name for the field in the requisite.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used ||
|| **SORT**
[`integer`](../../../../data-types.md) | Sorting. The order in the list of template fields ||
|| **IN_SHORT_LIST**
[`char`](../../../../data-types.md) | Show in the short list. Deprecated field, currently not used. Retained for backward compatibility. Can take values `Y` or `N` ||
|#

Use the method [crm.requisite.preset.field.fields](./crm-requisite-preset-field-fields.md) to get a formal description of the fields.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [crm.requisite.preset.field.add](./crm-requisite-preset-field-add.md) | Adds a customizable field to the requisites template ||
|| [crm.requisite.preset.field.update](./crm-requisite-preset-field-update.md) | Modifies a customizable field in the requisites template ||
|| [crm.requisite.preset.field.availabletoadd](./crm-requisite-preset-field-available-to-add.md) | Returns fields available for addition to the specified requisites template ||
|| [crm.requisite.preset.field.get](./crm-requisite-preset-field-get.md) | Returns the description of a customizable field in the requisites template by ID ||
|| [crm.requisite.preset.field.list](./crm-requisite-preset-field-list.md) | Returns a list of all customizable fields for a specific requisites template ||
|| [crm.requisite.preset.field.delete](./crm-requisite-preset-field-delete.md) | Deletes a customizable field from the requisites template ||
|| [crm.requisite.preset.field.fields](./crm-requisite-preset-field-fields.md) | Returns a formal description of a configurable template field ||
|#
