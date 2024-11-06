# Statuses and dropdowns in CRM: Overview of Methods

In the detail forms of CRM entities, there are two types of "list" fields:

* Custom fields — these can be created, modified, and deleted using the methods crm.xx.userfield.*. For example, to create a custom list field in deals, use [crm.deal.userfield.add](../deals/user-defined-fields/crm-deal-userfield-add.md)

* System fields — these are pre-installed and cannot be created or deleted. In system list fields, only the values of the list can be modified. The list of values for system fields of type "list" is called a reference book in CRM.

> Quick navigation: [all methods and events](#all-methods)
> 
> User documentation: [Statuses and dropdowns in Bitrix24](https://helpdesk.bitrix24.com/open/21656894/)

## List of Statuses and dropdowns

To modify a reference book, specify the `ENTITY_ID` parameter in the methods of the group crm.status.*. If the value is incorrect, another reference book will be modified.

**Stages.** A reference book with stages of working with clients, which are displayed in the CRM kanban. Each CRM object has its own code for the stages reference book:
* [Deals](../deals/index.md) — `DEAL_STAGE` for the main direction of deals and `DEAL_STAGE_xx` for additional, where xx is the ID of the direction
* [Leads](../leads/index.md) — `STATUS`
* [Invoices](../universal/invoice.md) — `SMART_INVOICE_STAGE_xx`, where xx is the ID value of the invoice direction
* [Estimates](../quote/index.md) — `QUOTE_STATUS`
* [Documents](https://helpdesk.bitrix24.com/open/17612480/) — `SMART_DOCUMENT_STAGE_xx`, where xx is the ID value of the document direction
* [SPAs](../universal/index.md) — `DYNAMIC_xx_STAGE_xx`, where the first xx is the ID of the SPA, and the second xx is the ID of the direction

To get the ID of the direction for a specific Bitrix24, use the method [crm.status.entity.types](./crm-status-entity-types.md).

**Sources.** A reference book with values for the system field Source — `SOURCE`.

**Contact and Company Types.** Reference books with values for the system fields Contact Type — `CONTACT_TYPE`, Company Type — `COMPANY_TYPE`.

**Number of Employees.** A reference book with values for the system field Number of Employees in the company detail form — `EMPLOYEES`.

**Client Industry.** A reference book with values for the system field Industry in the company detail form — `INDUSTRY`.

**Deal Type.** A reference book with values for the system field Deal Type in the deal detail form — `DEAL_TYPE`.

**Honorifics.** A reference book with values for the system field Honorific in the lead and contact detail forms — `HONORIFIC`.

**Call Statuses.** A reference book with values for the Call Status in the call list detail form — `CALL_LIST`.

## How to Use Reference Book Values

Each list value in the reference books has:

* name `NAME` — displayed in the CRM entity detail form
* status `STATUS_ID` — used in methods for creating and modifying entities

Using the method [crm.deal.update](../deals/crm-deal-update.md), you can change the values of the fields `Deal Stage` — `STAGE_ID` and `Source` — `SOURCE_ID`. Both fields are system list fields.

To display the name of the stage or source in the CRM detail form, it is important to pass not the `NAME` of the value, but its `STATUS_ID` in the method. For the stage "New Request," this may be `C1:NEW`, and for the source "Call" — `CALL`.

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [crm.status.add](./crm-status-add.md) | Creates a new element in the specified reference book ||
|| [crm.status.delete](./crm-status-delete.md) | Deletes an element from the reference book ||
|| [crm.status.entity.items](./crm-status-entity-items.md) | Returns elements of the reference book by its symbolic identifier ||
|| [crm.status.entity.types](./crm-status-entity-types.md) | Returns descriptions of reference book types ||
|| [crm.status.fields](./crm-status-fields.md) | Returns descriptions of reference book fields ||
|| [crm.status.get](./crm-status-get.md) | Returns an element of the reference book by its identifier ||
|| [crm.status.list](./crm-status-list.md) | Returns a list of elements of the reference book by filter ||
|| [crm.status.update](./crm-status-update.md) | Updates an existing element of the reference book ||
|#