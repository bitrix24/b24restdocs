# Data Import into CRM

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "import" access permission for the CRM entity

Import supports all major [CRM entities](../../data-types.md#object_type):

- [Leads](../../leads/index.md)
- [Deals](../../deals/index.md)
- [Contacts](../../contacts/index.md)
- [Companies](../../companies/index.md)
- [Estimates](../../quote/index.md)
- Invoices
- [SPAs](../user-defined-object-types/index.md)

Two methods are available for import:

- [`crm.item.import`](./crm-item-import.md) — import a single record
- [`crm.item.batchImport`](./crm-item-batch-import.md) — batch import of records

Adding entities through import, unlike adding through [`crm.*.add`](../crm-item-add.md), has the following features:

- Access permission checks are performed on import, not on adding entities.
- Automation rules and workflows will not be triggered on the added entity.
- When executed under the portal administrator, there is an option to set values for certain system fields:

#|
|| **Field** | **Description** ||
|| **createdTime** | Date and time of record creation ||
|| **updatedTime** | Date and time of record update ||
|| **movedTime** | Date and time of stage change (if the entity supports stages) ||
|| **createdBy** | User who created the record ||
|| **updatedBy** | User who modified the record ||
|| **movedBy** | User who changed the stage (if the entity supports stages) ||
|#

There are some restrictions on the values of these fields:

- A monotonically increasing value for the `createdTime` field is required. This means that records cannot be created with a creation date earlier than any existing records.
- The values of `createdTime` and `updatedTime` cannot be in the future.
- The `updatedTime` cannot be earlier than `createdTime`.
- The `movedTime` must be within the range between `createdTime` and `updatedTime`.