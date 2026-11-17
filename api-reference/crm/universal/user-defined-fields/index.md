# Custom Fields in CRM: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Custom fields allow you to extend CRM cards to fit your own processes: adding additional attributes, storing internal classifiers, and transferring data between the CRM and external systems.

The methods `crm.userfield.*` help understand the structure of a custom field: its characteristics, types, settings, and format of list values. They do not create a field in the object card but return reference information for configuring it.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## How to Choose a Section {#how-to-choose}

The documentation has several sections about custom fields. Choose the one that matches your task.

#|
|| **If You Need To** | **Open the Section** ||
|| Learn the types, characteristics, settings, and list format | The current section ||
|| Create, modify, or delete a field of a lead, deal, contact, company, estimate, invoice, or SPA in a new integration | [CRM Fields in REST 3.0](../../rest-v3/fields/index.md), the `crm.<type>.field.custom.*` methods ||
|| Create a field of the `enumeration` or `file` type or of an application type | [Custom Field Settings](../userfieldconfig/index.md) or the object method `crm.*.userfield.add`. REST 3.0 does not create such fields ||
|| Create a requisite field or maintain an integration built on object methods | The `crm.*.userfield.*` sections: [leads](../../leads/userfield/index.md), [deals](../../deals/user-defined-fields/index.md), [contacts](../../contacts/userfield/index.md), [companies](../../companies/userfields/index.md), [estimates](../../quote/user-field/index.md), [requisites](../../requisites/user-fields/index.md). The methods for leads, deals, contacts, companies, and estimates are deprecated ||
|| Display your application's interface in a field | [Custom Field Types in CRM](./userfield-type.md) ||
|#

## Connection with CRM Objects

**CRM Objects.** The methods of this section do not depend on the object: characteristics, types, and settings are the same for all CRM objects. The object is set when the field is created. The object methods `crm.*.userfield.add` fill in `ENTITY_ID` themselves. In REST 3.0, the object is specified in the method name, for example `crm.deal.field.custom.add`.

## Key Terms {#key-terms}

- `ENTITY_ID` — code of the CRM object to which the field is linked, for example `CRM_DEAL`
- `FIELD_NAME` — code of the custom field in the format `UF_CRM_*`
- `USER_TYPE_ID` — data type of the custom field; a complete list of values is returned by [crm.userfield.types](./crm-userfield-types.md)
- `SETTINGS` — field settings that depend on the selected type
- `LIST` — set of elements for the `enumeration` type field
- `MULTIPLE` — multiple field flag: `Y` or `N`. [crm.item.add](../crm-item-add.md) and [crm.item.update](../crm-item-update.md) accept the values of such a field as an array, even if there is only one value. [crm.item.get](../crm-item-get.md) also returns them as an array

## How to Get Started

1. Retrieve available types through [crm.userfield.types](./crm-userfield-types.md) and select the appropriate `USER_TYPE_ID`.
2. Choose the method for creating the field using the [How to Choose a Section](#how-to-choose) table: the method depends on the field type and the object.
3. Retrieve the field structure via [crm.userfield.fields](./crm-userfield-fields.md) to find out which characteristics are set only on creation and which ones the system fills in itself.
4. For the selected type, retrieve the settings through [crm.userfield.settings.fields](./crm-userfield-settings-fields.md).
5. If the type is `enumeration`, check the format of list elements through [crm.userfield.enumeration.fields](./crm-userfield-enumeration-fields.md).
6. Create the field with the chosen method. In REST 3.0, field and settings keys are passed in camelCase: `userTypeId` instead of `USER_TYPE_ID`, `iblockId` instead of `IBLOCK_ID`.

{% note tip "Additionally" %}

- [{#T}](../../../../tutorials/field-types/how-to-add-user-field-to-spa.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [crm.userfield.types](./crm-userfield-types.md) | Returns a list of custom field types ||
|| [crm.userfield.fields](./crm-userfield-fields.md) | Returns a description of custom field characteristics ||
|| [crm.userfield.settings.fields](./crm-userfield-settings-fields.md) | Returns a description of the settings for the specified type ||
|| [crm.userfield.enumeration.fields](./crm-userfield-enumeration-fields.md) | Returns a description of fields for the custom field of type `enumeration` ||
|#

## Continue Learning

- [{#T}](./userfield-type.md)
- [{#T}](../userfieldconfig/index.md)
- [{#T}](../index.md)