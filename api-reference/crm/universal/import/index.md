# Mass Data Import into CRM

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits to meet writing standards

{% endnote %}

{% endif %}

The import supports all major [CRM entities](../../data-types.md#object_type):

- Leads
- Deals
- Contacts
- Companies
- Estimates
- Invoices
- SPAs

Two methods are available for import:

- [`crm.item.import`](crm-item-import.md) - Import a single record
- [`crm.item.batchImport`](crm-item-batch-import.md) - Batch import of records

Adding entities through import, as opposed to adding via [`crm.*.add`](../crm-item-add.md), has the following features:

- Access permissions are checked during import, not when adding entities;
- Automation rules and workflows will not be triggered on the added entity;
- When executed under the portal administrator, there is an option to set values for certain system fields:

#|
|| **Field** | **Description** ||
|| **createdTime** | Date and time the record was created ||
|| **updatedTime** | Date and time the record was updated ||
|| **movedTime** | Date and time the stage was changed (if the entity supports stages) ||
|| **createdBy** | User who created the record ||
|| **updatedBy** | User who modified the record ||
|| **movedBy** | User who changed the stage (if the entity supports stages) ||
|#

There are some restrictions on the values of these fields:

- A monotonically increasing value for the `createdTime` field is required. This means that records cannot be created with a creation date earlier than any existing records;
- `createdTime` and `updatedTime` cannot be in the future;
- `updatedTime` cannot be less than `createdTime`;
- `movedTime` must be within the range between `createdTime` and `updatedTime`.