# Custom Field Settings: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods in this section allow you to manage custom fields: access available field types, create a field for the desired object, read its settings, update parameters, and delete the field.

Technically, the handler for these methods resides in the `main` module, but in REST, they are available as a separate group `userfieldconfig.*` and use the scope `userfieldconfig`. In AJAX, the same handler is called as `main.userfieldconfig.*`.

{% note info "" %}

In all `userfieldconfig.*` methods, `moduleId` is passed, which determines the module context and access permissions for the fields. Therefore, the application requires not only the `userfieldconfig` scope but also the module scope from `moduleId`. For example, to modify fields in the Business Automation module, both `userfieldconfig` and `rpa` scopes are needed.

{% endnote %}

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## Getting Started

1. Determine the `moduleId` and the format of `entityId` for the desired object.
2. Access available field types through [userfieldconfig.getTypes](./userfieldconfig-get-types.md).
3. Create a field using the [userfieldconfig.add](./userfieldconfig-add.md) method.
4. Retrieve the list of custom field settings through [userfieldconfig.list](./userfieldconfig-list.md) or the settings of a specific field through [userfieldconfig.get](./userfieldconfig-get.md).
5. If necessary, modify or delete the field using the [userfieldconfig.update](./userfieldconfig-update.md) and [userfieldconfig.delete](./userfieldconfig-delete.md) methods.

## Relationships with Other Objects

**CRM.** The relationship of fields with CRM objects is defined by the pair `moduleId=crm` and the `entityId` of the specific CRM object. For smart processes, obtain the `ID` using the [crm.type.list](../user-defined-object-types/crm-type-list.md) method, then pass the `entityId`. For other objects, such as leads, contacts, and companies, use fixed identifiers.

**RPA.** When working with Business Automation processes, use `moduleId=rpa`. Pass the process identifier in `entityId`.

{% note info "" %}

The `userfieldconfig.*` methods also work in other modules if the module supports custom fields. For example, for inventory management, use `moduleId=catalog` and the format `entityId` for warehouse documents. For details, see the section [Custom Fields for Inventory Documents](../../../catalog/userfield-document/index.md).

{% endnote %}

## Identifiers for entityId {#entity-id}

The association of a field with an object is defined by the value of `entityId`.

### CRM with Fixed Identifiers

- `CRM_LEAD` — lead
- `CRM_CONTACT` — contact
- `CRM_COMPANY` — company
- `CRM_DEAL` — deal
- `CRM_QUOTE` — estimate
- `CRM_INVOICE` — invoice
- `CRM_SMART_INVOICE` — new invoice
- `CRM_REQUISITE` — requisite
- `CRM_SMART_DOCUMENT` — CRM document
- `CRM_SMART_B2E_DOC` — B2E document

### CRM Smart Processes

For a smart process, the format `CRM_{ID}` is used, where `ID` is the identifier of the smart process.

{% note tip "Typical use-cases and scenarios" %}

- [How to Create a Custom Field in a Smart Process](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-user-field-to-spa.md)

{% endnote %}

### RPA

In the `rpa` module, the format `RPA_{ID}` is used, where `ID` is the process identifier.

## Overview of Methods {#all-methods}

> Scope: [`userfieldconfig`](../../../scopes/permissions.md), module scope from `moduleId` (e.g., [`crm`](../../../scopes/permissions.md))
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [userfieldconfig.add](./userfieldconfig-add.md) | Creates a custom field ||
|| [userfieldconfig.update](./userfieldconfig-update.md) | Updates the settings of a custom field ||
|| [userfieldconfig.get](./userfieldconfig-get.md) | Retrieves the settings of a custom field by identifier ||
|| [userfieldconfig.list](./userfieldconfig-list.md) | Retrieves a list of custom field settings ||
|| [userfieldconfig.delete](./userfieldconfig-delete.md) | Deletes a custom field ||
|| [userfieldconfig.getTypes](./userfieldconfig-get-types.md) | Retrieves available types of custom fields for the module ||
|#