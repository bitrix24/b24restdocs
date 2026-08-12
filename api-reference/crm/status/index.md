# Reference Guides in CRM: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In the detail forms of CRM entities, there are two types of list fields:

* Custom fields — these can be created, modified, and deleted using the methods crm.xx.userfield.*. For example, to create a custom list field in deals, use [crm.deal.userfield.add](../deals/user-defined-fields/crm-deal-userfield-add.md).

* System fields — these are pre-installed and cannot be created or deleted. In system list fields, only the values of the list can be modified. The list of values for system list fields is referred to as a reference guide in CRM.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Statuses and dropdowns in CRM](https://helpdesk.bitrix24.com/open/21656894/)

## Getting Started

1. Retrieve the list of reference guide types using the [crm.status.entity.types](./crm-status-entity-types.md) method — it returns `ENTITY_ID` codes, for example `DEAL_STAGE` or `SOURCE`
2. Review the current values of a reference guide using the [crm.status.entity.items](./crm-status-entity-items.md) method or the [crm.status.list](./crm-status-list.md) method with a filter by `ENTITY_ID`
3. Add a new value using the [crm.status.add](./crm-status-add.md) method: pass `ENTITY_ID`, a unique `STATUS_ID` code, and a `NAME` to it
4. Change the name, color, or sorting of a value using the [crm.status.update](./crm-status-update.md) method, and delete an unnecessary one using the [crm.status.delete](./crm-status-delete.md) method
5. Use `STATUS_ID` in the methods of CRM objects, for example in [crm.deal.update](../deals/crm-deal-update.md)

## List of Reference Guides

To modify a reference guide, specify the `ENTITY_ID` parameter in the methods of the group crm.status.*. If the value is incorrect, another reference guide will be modified.

**Stages.** A reference guide with stages of working with clients, which are displayed in the CRM kanban. Each CRM object has its own code for the stages reference guide:
* [Deals](../deals/index.md) — `DEAL_STAGE` for the main deal pipeline and `DEAL_STAGE_xx` for an additional one, where xx is the ID of the pipeline.
* [Leads](../leads/index.md) — `STATUS`
* [Invoices](../universal/invoice.md) — `SMART_INVOICE_STAGE_xx`, where xx is the ID value of the invoice pipeline.
* [Quotes](../quote/index.md) — `QUOTE_STATUS`
* [Documents](https://helpdesk.bitrix24.com/open/19441484/) — `SMART_DOCUMENT_STAGE_xx`, where xx is the ID value of the document pipeline.
* [Smart Processes](../universal/index.md) — `DYNAMIC_xx_STAGE_xx`, where the first xx is the `entityTypeId` of the smart process, and the second xx is the ID of the pipeline.

To obtain the ID of a specific pipeline in Bitrix24, use the method [crm.status.entity.types](./crm-status-entity-types.md).

**Sources.** A reference guide with values for the system field Source — `SOURCE`.

**Contact and Company Types.** Reference guides with values for the system fields Contact Type — `CONTACT_TYPE` and Company Type — `COMPANY_TYPE`.

**Number of Employees.** A reference guide with values for the system field Number of Employees in the company detail form — `EMPLOYEES`.

**Client Industry.** A reference guide with values for the system field Industry in the company detail form — `INDUSTRY`.

**Deal Type.** A reference guide with values for the system field Deal Type in the deal detail form — `DEAL_TYPE`.

**Honorifics.** A reference guide with values for the system field Honorific in the lead and contact detail forms — `HONORIFIC`.

**Call Statuses.** A reference guide with values for the Call Status in the call list detail form — `CALL_LIST`.

## How to Use Reference Guide Values

Each list value in the reference guides has:

* name `NAME` — displayed in the CRM object detail form
* status `STATUS_ID` — used in methods for creating and modifying entities

Using the method [crm.deal.update](../deals/crm-deal-update.md), you can change the values of the fields `Deal Stage` — `STAGE_ID` and `Source` — `SOURCE_ID`. Both fields are system list fields.

To display the name of the stage or source in the CRM detail form, it is important to pass the `STATUS_ID` of the value, not the `NAME`. For the stage "New Request," this might be `C1:NEW`, and for the source "call," it would be `CALL`.

Some reference guide values are system ones — their `SYSTEM` field contains `Y`. A system value cannot be created via REST: the [crm.status.add](./crm-status-add.md) method always saves a new element with `SYSTEM` = `N`. A system value can only be deleted with the `FORCED` flag in the [crm.status.delete](./crm-status-delete.md) method.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

Any user with access to CRM can read reference guides. Only an administrator or a user with the "Allow to modify settings" access permission in CRM can add, update, and delete reference guide elements.

#| 
|| **Method** | **Description** ||
|| [crm.status.add](./crm-status-add.md) | Creates a new element in the specified reference guide ||
|| [crm.status.update](./crm-status-update.md) | Updates an existing element of the reference guide ||
|| [crm.status.get](./crm-status-get.md) | Returns an element of the reference guide by its identifier ||
|| [crm.status.list](./crm-status-list.md) | Returns a list of elements from the reference guide based on a filter ||
|| [crm.status.delete](./crm-status-delete.md) | Deletes an element from the reference guide ||
|| [crm.status.entity.items](./crm-status-entity-items.md) | Returns elements of the reference guide by its symbolic identifier ||
|| [crm.status.entity.types](./crm-status-entity-types.md) | Returns descriptions of reference guide types ||
|| [crm.status.fields](./crm-status-fields.md) | Returns descriptions of reference guide fields ||
|#
