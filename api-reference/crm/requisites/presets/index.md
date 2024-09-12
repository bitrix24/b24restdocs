# About Requisite Templates

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute methods: any user

## Fields of the Requisite Template

#|
|| **Name** | **Description** | **Read** | **Write** | **Required** | **Immutable** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the requisite. Automatically created and unique within the account | Yes | No | No | Yes ||
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
[`user`](../../../data-types.md) | Identifier of the user who created the requisite | Yes | No | No | No ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the requisite | Yes | No | No | No ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the requisite | Yes | Yes | Yes | No ||
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

## Overview of Methods

#|
|| **Method** | **Description** ||
|| [crm.requisite.preset.add](./crm-requisite-preset-add.md) | Creates a new requisite template ||
|| [crm.requisite.preset.update](./crm-requisite-preset-update.md) | Modifies a requisite template ||
|| [crm.requisite.preset.countries](./crm-requisite-preset-countries.md) | Returns a possible list of countries for requisite templates ||
|| [crm.requisite.preset.get](./crm-requisite-preset-get.md) | Returns a requisite template by identifier ||
|| [crm.requisite.preset.list](./crm-requisite-preset-list.md) | Returns a list of requisite templates by filter ||
|| [crm.requisite.preset.delete](./crm-requisite-preset-delete.md) | Deletes a requisite template ||
|| [crm.requisite.preset.fields](./crm-requisite-preset-fields.md) | Returns a formal description of the fields of the requisite template ||
|#