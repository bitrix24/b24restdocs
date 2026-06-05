# Data Import into CRM: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Importing is used when there is a need to transfer existing data into the CRM. Import methods do not automatically fetch data. The application prepares the data according to the field structure of Bitrix24 CRM and sends it in a REST request.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Transfer data between different accounts](https://helpdesk.bitrix24.com/open/25825927/)

## What Objects Can Be Imported

Before importing, specify which objects' data needs to be transferred to the CRM. To do this, pass the `entityTypeId` in the request — a numeric code representing the CRM object where Bitrix24 will save this information.

If you are transferring deals, pass `2`; for contacts — `3`; for new invoices — `31`. A complete list of system values can be found in the [CRM object types reference](../../data-types.md#object_type).

Import is available for the main CRM objects:

- [leads](../../leads/index.md)
- [deals](../../deals/index.md)
- [contacts](../../contacts/index.md)
- [companies](../../companies/index.md)
- [estimates](../../quote/index.md)
- [invoices](../invoice.md)
- [SPAs](../user-defined-object-types/index.md)

## How to Get Started

1. Select the CRM object whose data needs to be transferred and pass its code in `entityTypeId`.
2. Retrieve the list of fields for the selected CRM object using the [crm.item.fields](../crm-item-fields.md) method or the method for a specific CRM object: [crm.lead.fields](../../leads/crm-lead-fields.md), [crm.deal.fields](../../deals/crm-deal-fields.md), [crm.contact.fields](../../contacts/crm-contact-fields.md), [crm.company.fields](../../companies/crm-company-fields.md), [crm.quote.fields](../../quote/crm-quote-fields.md).
3. Pass the data for a single item using the [crm.item.import](./crm-item-import.md) method or up to 20 items using the [crm.item.batchImport](./crm-item-batch-import.md) method.
4. If you are importing custom fields, choose the format of their names via `useOriginalUfNames`: original names like `UF_CRM_2_1639669411830` or camelCase names like `ufCrm2_1639669411830`.

## Important Considerations

- Importing differs from the usual creation of an item: Bitrix24 checks the user's permission for import, not just the permission for regular addition.
- If automation is set up in the CRM upon item creation, the import will not trigger it. Automation rules and workflows will not execute actions for items created through import.

## How to Preserve Item History

When an item is created normally, Bitrix24 automatically fills in the creation date, modification date, and users who performed actions. During import, the administrator can pass these values in the request to preserve the history from the external system.

System fields are passed within the data of the imported item: in the `fields` of the [crm.item.import](./crm-item-import.md) method or in the elements of the `data` array of the [crm.item.batchImport](./crm-item-batch-import.md) method.

For example, if a deal was created in an external CRM on January 10, 2024, pass `createdTime` with that date. Then, in Bitrix24, the imported deal will retain the original creation date instead of the import execution date.

#| 
|| **Field** | **What it preserves** ||
|| `createdTime` | The date and time the item was created ||
|| `updatedTime` | The date and time the item was last modified ||
|| `movedTime` | The date and time the stage was changed, if the object supports stages ||
|| `createdBy` | The user who created the item ||
|| `updatedBy` | The user who modified the item ||
|| `movedBy` | The user who changed the stage, if the object supports stages ||
|#

There are restrictions for dates:

- Import items in ascending order of `createdTime`: older ones first, then newer ones. You cannot import an item with a creation date earlier than already created items.
- `createdTime` and `updatedTime` must not be later than the current date and time.
- `updatedTime` must be equal to or later than `createdTime`.
- `movedTime` must fall within the period between `createdTime` and `updatedTime`.

## Linking to Other Objects

**CRM Items.** Import creates items of a specific CRM type. The link is established through `entityTypeId` in the [crm.item.import](./crm-item-import.md) and [crm.item.batchImport](./crm-item-batch-import.md) methods. The identifier of the created item is returned in the response of the method.

**CRM Fields.** The set of fields depends on the type of CRM object. Before importing, obtain the field description using the universal method [crm.item.fields](../crm-item-fields.md) or the fields method for the specific CRM object.

**SPAs.** To import SPA items, pass its identifier in `entityTypeId`. You can obtain the identifier using the [crm.type.list](../user-defined-object-types/crm-type-list.md) method.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with permission to import an item of the CRM object

#| 
|| **Method** | **Description** ||
|| [crm.item.import](./crm-item-import.md) | Imports a single item ||
|| [crm.item.batchImport](./crm-item-batch-import.md) | Imports a group of items ||
|#