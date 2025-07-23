# CRM Vendors: Overview of Methods

{% note info "" %}

CRM vendors are available starting from version 23.0.0 of the Trade Catalog module and REST API 23.100.0.

{% endnote %}

Vendors are individuals and legal entities that supply goods. In CRM, they are added as contacts and companies, but they do not participate in deals, leads, or estimates. CRM vendor data is only indicated in stock receipt documents. Multiple contacts and one company can be attached to a single document.

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [Vendors](https://helpdesk.bitrix24.com/open/16764476/)

## Linking Vendors with Other Objects

**Inventory Management.** Vendors are indicated in stock receipt documents. To work with documents, use the methods [catalog.document.\*](../document/index.md).

**CRM.** Vendors are CRM entities. To work with them, use the universal methods [crm.item.\*](../../crm/universal/index.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [catalog.documentcontractor.add](./catalog-documentcontractor-add.md) | Adds a vendor binding to the document ||
|| [catalog.documentcontractor.delete](./catalog-documentcontractor-delete.md) | Removes the vendor binding from the document by binding ID ||
|| [catalog.documentcontractor.getFields](./catalog-documentcontractor-get-fields.md) | Returns a list of vendor bindings to documents based on the filter ||
|| [catalog.documentcontractor.list](./catalog-documentcontractor-list.md) | Returns fields with CRM vendor bindings to inventory documents ||
|#