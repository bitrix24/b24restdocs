# Universal CRM Methods: Overview of Methods and Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods `crm.item.*` manage CRM entities: leads, deals, contacts, companies, invoices, estimates, and SPA elements.

A unified interface simplifies working with different entities. Instead of separate commands for each entity, use universal methods with the type identifier `entityTypeId`.

> Quick navigation: [all methods and events](#all-methods)

## Getting Started

1. Determine the CRM entity type `entityTypeId`. Obtain the value from the [CRM object types directory](../../data-types.md#object_type) or the [SPA section](./user-defined-object-types/index.md).
2. Retrieve the list of available fields for this type using the [crm.item.fields](./crm-item-fields.md) method.
3. Create a new entity using the [crm.item.add](./crm-item-add.md) method or get a list of existing entities using the [crm.item.list](./crm-item-list.md) method.
4. Retrieve data for a specific entity using the [crm.item.get](./crm-item-get.md) method.
5. Modify the entity using the [crm.item.update](./crm-item-update.md) method or delete it using the [crm.item.delete](./crm-item-delete.md) method.

## Relationships of Universal Methods with Other Entities

**CRM Entity Type.** The type is defined by the `entityTypeId` parameter. It determines the structure of fields and the logic of the method.

**CRM Entity.** A specific record is identified by the pair `entityTypeId` and `id`. This combination is used in the core methods `crm.item.*`.

**Requisites.** Link company and contact requisites using the [crm.requisite.link.*](../requisites/links/index.md) methods.

**Parent Relationships.** Entities are linked through the `parentId` and `parentEntityTypeId` fields:

- `fields` returns information about parent fields
- `get` provides values for parent fields
- `list` filters, sorts, and adds parent field values to the selection
- `add` and `update` support changing the values of these fields

## Field Naming Conventions

In the database, fields are stored in `UPPER_CASE` format, while in REST, names are used in `camelCase`.

Example: `ASSIGNED_BY_ID` becomes `assignedById`.

For custom fields, the conversion is more complex because original names often contain numbers and underscores.

Standard case: `UF_CRM_10_5186744711` becomes `ufCrm10_5186744711`. Underscores between numeric blocks are preserved, while others are removed.

A problem arises when letters and numbers are mixed. For example, `UF_CRM_10_DIGIT10` converts to `ufCrm10Digit10`. During reverse conversion, it is impossible to determine whether the original field was `UF_CRM_10_DIGIT10` or `UF_CRM_10_DIGIT_10`.

Starting from **CRM 21.1800.0**, the system checks for such conflicts before conversion. If the name becomes ambiguous, it is returned in its original `UPPER_CASE` form.

#|
|| **UPPER_CASE** | **camelCase** ||
|| `UF_CRM_3_DIGIT` | `ufCrm3Digit` ||
|| `UF_CRM_3_DIGIT10` | `ufCrm_3_DIGIT10` ||
|| `UF_CRM_3_DIGIT_10` | `ufCrm_3_DIGIT_10` ||
|| `UF_CRM_3_1747309727` | `ufCrm3_1747309727` ||
|| `UF_CRM_1747309879` | `ufCrm_1747309879` ||
|#

Starting from **CRM 25.0.0**, the methods `crm.item.*` support the `useOriginalUfNames` parameter, which controls the format of custom field names in requests and responses:

- `Y` — original custom field names, e.g., `UF_CRM_2_1639669411830`
- `N` — custom field names in `camelCase`, e.g., `ufCrm2_1639669411830`

By default, `N` is used.

## Use Cases

- Configure the entity structure and fields: [CRM Object Fields](./object-fields.md), [Custom Fields](./user-defined-fields/index.md), [Custom Field Settings](./userfieldconfig/index.md)
- Work with SPAs and stages: [SPAs](./user-defined-object-types/index.md), [CRM Funnels](./category/index.md)
- Manage the composition of a deal or invoice: [Product Items](./product-rows/index.md), [Payments and Deliveries](./payment/index.md), [Deliveries](./delivery/index.md), [Invoices](./invoice.md)
- Configure the detail form and interface: [Manage Item Detail Forms](./item-details-configuration/index.md), [Widgets](./widgets.md)
- Bulk upload and link data: [Import](./import/index.md), [Link CRM with Online Store Orders](./order-entity/index.md)

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute methods: depending on the method

{% list tabs %}

- Methods

  #|
  || **Method** | **Description** ||
  || [crm.item.add](./crm-item-add.md) | Creates a CRM entity ||
  || [crm.item.update](./crm-item-update.md) | Modifies fields of a CRM entity ||
  || [crm.item.get](./crm-item-get.md) | Returns data of a CRM entity by identifier ||
  || [crm.item.list](./crm-item-list.md) | Returns a list of CRM entities ||
  || [crm.item.delete](./crm-item-delete.md) | Deletes a CRM entity ||
  || [crm.item.fields](./crm-item-fields.md) | Returns the description of CRM entity fields ||
  |#

- Events

  #|
  || **Event** | **Triggered By** ||
  || [onCrmDynamicItemAdd](./events/on-crm-dynamic-item-add.md) | After adding a SPA entity ||
  || [onCrmDynamicItemUpdate](./events/on-crm-dynamic-item-update.md) | After updating a SPA entity ||
  || [onCrmDynamicItemDelete](./events/on-crm-dynamic-item-delete.md) | After deleting a SPA entity ||
  |#

{% endlist %}

## Continue Learning

- [{#T}](./invoice.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-user-field-to-spa.md)