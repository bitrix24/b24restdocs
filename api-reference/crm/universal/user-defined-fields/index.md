# Custom Fields in CRM: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields allow you to extend CRM cards to fit your own processes: adding additional attributes, storing internal classifiers, and transferring data between the CRM and external systems.

The methods `crm.userfield.*` help understand the structure of a custom field: its characteristics, types, settings, and format of list values. They do not create a field in the object card but return reference information for configuring it.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## How to Choose a Section

The documentation has several sections about custom fields. Choose the one that matches your task.

#|
|| **If You Need To** | **Open the Section** ||
|| Learn the types, characteristics, settings, and list format | The current section ||
|| Create, modify, or delete a field for any object | [Custom Field Settings](../userfieldconfig/index.md) ||
|| Create a field for a specific object using its own methods | The `crm.*.userfield.*` sections: [leads](../../leads/userfield/index.md), [deals](../../deals/user-defined-fields/index.md), [contacts](../../contacts/userfield/index.md), [companies](../../companies/userfields/index.md), [estimates](../../quote/user-field/index.md), [requisites](../../requisites/user-fields/index.md) ||
|| Display your application's interface in a field | [Custom Field Types in CRM](./userfield-type.md) ||
|#

## Connection of Custom Fields with CRM Objects

**CRM Objects.** Object methods `crm.*.userfield.*` use reference information from this section for [leads](../../leads/userfield/index.md), [deals](../../deals/user-defined-fields/index.md), [contacts](../../contacts/userfield/index.md), [companies](../../companies/userfields/index.md), [estimates](../../quote/user-field/index.md), and [requisites](../../requisites/user-fields/index.md).

## Key Terms

- `ENTITY_ID` — code of the CRM object to which the field is linked
- `FIELD_NAME` — code of the custom field in the format `UF_CRM_*`
- `USER_TYPE_ID` — data type of the custom field; a complete list of values is returned by [crm.userfield.types](./crm-userfield-types.md)
- `SETTINGS` — field settings that depend on the selected type
- `LIST` — set of elements for the `enumeration` type field

## How to Get Started

1. Identify the CRM object for which you need to add a field and open its section `crm.*.userfield.*`. For example, [crm.lead.userfield.*](../../leads/userfield/index.md), [crm.deal.userfield.*](../../deals/user-defined-fields/index.md), [crm.contact.userfield.*](../../contacts/userfield/index.md).
2. Retrieve available types through [crm.userfield.types](./crm-userfield-types.md) and select the appropriate `USER_TYPE_ID`.
3. Obtain the field structure via [crm.userfield.fields](./crm-userfield-fields.md) to check the required characteristics.
4. For the selected type, get the settings through [crm.userfield.settings.fields](./crm-userfield-settings-fields.md), where the `type` parameter for standard types matches `USER_TYPE_ID`.
5. If the type is `enumeration`, check the format of list elements through [crm.userfield.enumeration.fields](./crm-userfield-enumeration-fields.md).
6. After that, create or update the field in the object section.

{% note tip "Additionally" %}

- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-user-field-to-spa.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [crm.userfield.types](./crm-userfield-types.md) | Returns a list of custom field types ||
|| [crm.userfield.fields](./crm-userfield-fields.md) | Returns a description of custom field characteristics ||
|| [crm.userfield.settings.fields](./crm-userfield-settings-fields.md) | Returns a description of settings fields for the specified type ||
|| [crm.userfield.enumeration.fields](./crm-userfield-enumeration-fields.md) | Returns a description of fields for the custom field of type `enumeration` ||
|#

## Continue Learning

- [{#T}](./userfield-type.md)
- [{#T}](../userfieldconfig/index.md)
- [{#T}](../index.md)