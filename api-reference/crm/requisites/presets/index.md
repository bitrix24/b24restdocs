# CRM Requisite Templates: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A requisite template defines which fields a contact's or company's [requisite](../index.md) consists of. Every requisite is created from one of the templates, for example:
* the Company template consists of fields for legal entities such as LLC: `VAT ID`, `Company Name`
* the Person template consists of other fields: `First Name`, `Last Name`

The set of fields depends on the country: a template is linked to a country identifier, and only the fields supported by that country can be added to it.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to Create and Configure Company Details Templates in CRM](https://helpdesk.bitrix24.com/open/25770635/)

## Getting Started

1. Retrieve the list of countries using the [crm.requisite.preset.countries](./crm-requisite-preset-countries.md) method — a country identifier is required for a new template
2. Create a template using the [crm.requisite.preset.add](./crm-requisite-preset-add.md) method. Pass the parent object type, the country, and the name to it
3. Configure the set of template fields using the methods of the [customizable fields](./fields/index.md) section
4. Retrieve the list of templates using the [crm.requisite.preset.list](./crm-requisite-preset-list.md) method — the template identifier is required to create a requisite using the [crm.requisite.add](../universal/crm-requisite-add.md) method

## Template Identifiers

- `ID` — template identifier. It is returned by the [crm.requisite.preset.add](./crm-requisite-preset-add.md), [crm.requisite.preset.get](./crm-requisite-preset-get.md), and [crm.requisite.preset.list](./crm-requisite-preset-list.md) methods. This identifier is passed in the `PRESET_ID` field when creating a requisite
- `ENTITY_TYPE_ID` — parent object type. For requisite templates, it is always `8` — "Requisite". All values are provided by the [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) method
- `COUNTRY_ID` — country identifier. Available values are returned by the [crm.requisite.preset.countries](./crm-requisite-preset-countries.md) method
- `XML_ID` — external key for data exchange. Values of the form `#CRM_REQUISITE_PRESET_DEF_...` are reserved for default templates and must not be used for your own purposes

## Fields of the Requisite Template

#|
|| **Name** | **Description** | **Read** | **Write** | **Required** | **Immutable** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the template. Automatically created and unique within Bitrix24 | Yes | No | No | Yes ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object's type.

The identifiers of CRM object types are provided by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) | Yes | Yes | Yes | Yes ||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of fields in the requisite template (to get available values, see the method [crm.requisite.preset.countries](./crm-requisite-preset-countries.md)) | Yes | Yes | Yes | Yes ||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Creation date | Yes | No | No | No ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date. Contains an empty string if the template has not been changed since creation | Yes | No | No | No ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the template | Yes | No | No | No ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the template | Yes | No | No | No ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the template | Yes | Yes | Yes | No ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the external information base object.

The purpose of the field may change by the final developer.

Each application ensures the uniqueness of values in this field. It is recommended to use a unique prefix to avoid collisions with other applications.

Values of the form `#CRM_REQUISITE_PRESET_DEF_...` are reserved in CRM for identifying templates that are used by default. These identifiers should not be used for your purposes, as this may lead to logic violations | Yes | Yes | No | No ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status. Uses values `Y` or `N`. Determines the availability of the template in the selection list when adding requisites | Yes | Yes | No | No ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting | Yes | Yes | No | No ||
|#

Use the [crm.requisite.preset.fields](./crm-requisite-preset-fields.md) method to get a formal description of the fields.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: a user with access permissions for contacts and companies

Access permissions for templates are checked against two CRM objects at once — contacts and companies. To retrieve a template, "read" access permission for both objects is required. To create, modify, or delete a template, "add", "edit", or "delete" access permission for both objects is required.

#|
|| **Method** | **Description** ||
|| [crm.requisite.preset.add](./crm-requisite-preset-add.md) | Creates a new requisite template ||
|| [crm.requisite.preset.update](./crm-requisite-preset-update.md) | Modifies a requisite template ||
|| [crm.requisite.preset.get](./crm-requisite-preset-get.md) | Returns a requisite template by identifier ||
|| [crm.requisite.preset.list](./crm-requisite-preset-list.md) | Returns a list of requisite templates by filter ||
|| [crm.requisite.preset.delete](./crm-requisite-preset-delete.md) | Deletes a requisite template ||
|| [crm.requisite.preset.countries](./crm-requisite-preset-countries.md) | Returns the list of countries available for requisite templates ||
|| [crm.requisite.preset.fields](./crm-requisite-preset-fields.md) | Returns a formal description of the fields of the requisite template ||
|#