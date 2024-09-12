# Working with Custom Fields in the Requisite Template

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can perform the methods: any user

Custom fields are fields that can be added or removed from the template.

In the requisites editing form, in addition to the system fields that are present in any requisite (such as `Name` or `Comment`), there are other fields (like `Tax ID` and `KPP`) that are customizable. The methods in this section are designed to manage the list of such fields for a specific template.

Custom fields can be divided into two categories: pre-installed and user-defined.

The set of pre-installed fields is fixed for each supported country. Any such field can be added to a template related to the corresponding country if it is not already present in the template. Pre-installed fields have the prefix `RQ_`.

User-defined fields are added either by the user or through the corresponding [REST methods](../../user-fields/index.md). A created user-defined field can be added to a template. User-defined fields can be added to templates of any supported country. User-defined fields have the prefix `UF_`. If a user-defined field is used in templates for different countries, it is essential to ensure that it has the same meaning and name in the interface for them.

The **FIELD_NAME** field is essentially a reference to an existing field, either pre-installed or user-defined.

## Fields Describing the Custom Field in the Requisite Template

#|
||  **Name**
`type` | **Description** | **Read** | **Write** | **Required** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the field. Automatically created and unique within the template | Yes | No | No ||
|| **FIELD_NAME**
[`string`](../../../../data-types.md) | Name of the field | Yes | Yes | Yes ||
|| **FIELD_TITLE**
[`string`](../../../../data-types.md) | Alternative name for the field in the requisite.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used | Yes | Yes | No ||
|| **SORT**
[`integer`](../../../../data-types.md) | Sorting. Order in the list of fields in the template | Yes | Yes | No ||
|| **IN_SHORT_LIST**
[`char`](../../../../data-types.md) | Show in the short list. Deprecated field, currently not used. Retained for backward compatibility. Can take values `Y` or `N` | Yes | Yes | No ||
|#

Use the method [crm.requisite.preset.field.fields](./crm-requisite-preset-field-fields.md) to get a formal description of the fields.

## Overview of Methods

#|
|| **Method** | **Description** ||
|| [crm.requisite.preset.field.add](./crm-requisite-preset-field-add.md) | Adds a custom field to the requisites template ||
|| [crm.requisite.preset.field.update](./crm-requisite-preset-field-update.md) | Modifies a custom field in the requisites template ||
|| [crm.requisite.preset.field.availabletoadd](./crm-requisite-preset-field-available-to-add.md) | Returns fields available for addition to the specified requisites template ||
|| [crm.requisite.preset.field.get](./crm-requisite-preset-field-get.md) | Returns the description of the custom field in the requisites template by identifier ||
|| [crm.requisite.preset.field.list](./crm-requisite-preset-field-list.md) | Returns a list of all custom fields for a specific requisites template ||
|| [crm.requisite.preset.field.delete](./crm-requisite-preset-field-delete.md) | Removes a custom field from the requisites template ||
|| [crm.requisite.preset.field.fields](./crm-requisite-preset-field-fields.md) | Returns a formal description of the fields describing the custom field in the requisites template ||
|#