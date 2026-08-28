# Retrieving Lists in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so the assistant can utilize the official REST documentation.

{% endnote %}

CRM lists are data selections for integrations: duplicate clients by phone and email, addresses, activities related to deals, stages and pipelines, items at a selected stage, and vendors for inventory management.

A scenario is a sequence of requests for a single task. It describes the order of method calls, provides a code example, and states the result.

The tables below help you select a scenario by task, main methods, and result. Scenarios for creating CRM objects are collected in [How to Add Data to CRM](../how-to-add-crm-objects/index.md), and scenarios for changing data in [Editing Data in CRM](../how-to-edit-crm-objects/index.md).

## What You Need

**Scope.** All scenarios require the [`crm`](../../../api-reference/scopes/permissions.md) scope. Scenarios that display responsible employees additionally require access to user data — the [user.current](../../../api-reference/user/user-current.md) and [user.get](../../../api-reference/user/user-get.md) methods work with the [`user_brief`](../../../api-reference/scopes/permissions.md), `user_basic`, or `user` scope.

**Permissions.** The user needs permission to read the CRM items included in the selection. The methods return only the items and pipelines available to the user, so the same request produces a different result for different users.

**Result Pages.** List methods return up to 50 records per request. The remaining pages are retrieved with the `start` parameter — the calculation formula is provided on the method page, for example [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md).

The exact permissions and scope of a specific scenario are listed in the header of its page.

## How to Start

1. Choose a scenario in the table of the appropriate group
2. Create an [incoming webhook](../../../local-integrations/local-webhooks.md#incoming-webhook) or an application with the required scopes and check the user permissions
3. Execute the methods in the order described in the scenario
4. Use the retrieved identifiers in subsequent CRM or inventory management requests

## Finding Clients and Their Data

**Duplicates.** A single client can be entered in CRM several times — as a lead, a contact, and a company. The [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) method searches for matches by phone or email and returns identifiers grouped by object type. The data itself is then retrieved with the list methods of [leads](../../../api-reference/crm/leads/index.md), [contacts](../../../api-reference/crm/contacts/index.md), and [companies](../../../api-reference/crm/companies/index.md).

**Method Branch.** Development of the `crm.lead.*`, `crm.contact.*`, `crm.company.*`, and `crm.deal.*` methods has been discontinued. For new development, the same selections are retrieved with the [universal method](../../../api-reference/crm/universal/index.md) [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) and the required `entityTypeId`. Field names differ between the branches: the object methods work with `UPPER_CASE`, for example `STATUS_ID`, while the universal ones work with `camelCase`, for example `statusId`.

**Addresses.** A client address is stored in two independent ways: in the [requisites](../../../api-reference/crm/requisites/index.md) of a contact or a company and in a custom field of the `address` type. The two ways are not connected: an address from requisites is not visible in the custom field, and an address from the custom field is not visible to the `crm.address.*` methods. If it is unknown which way the address of a particular client is filled in, check both. A lead has no requisites — its address is linked to the lead itself.

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [How to Find Duplicates in CRM by Phone and Email](./search-by-phone-and-email.md) | [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md), [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md), [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md), [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) | A table of duplicates with the object type, name, phone number, and email ||
|| [How to Retrieve a Customer Address from the CRM](./how-to-get-address.md) | [crm.requisite.list](../../../api-reference/crm/requisites/universal/crm-requisite-list.md), [crm.address.list](../../../api-reference/crm/requisites/addresses/crm-address-list.md) | Client addresses from requisites and from a custom field ||
|#

## Working with Stages and Pipelines

The stage of a CRM item is stored as an identifier, not as a name. The two can be matched through [reference guides](../../../api-reference/crm/status/index.md) — system fields of the list type.

**Stage Code.** The [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method selects stages by the `ENTITY_ID` code, for example: `STATUS` for leads, `DEAL_STAGE` for the main deal pipeline, `DEAL_STAGE_{categoryId}` for an additional one, and `DYNAMIC_{entityTypeId}_STAGE_{categoryId}` for smart processes.

**Pipelines.** The pipeline identifier is returned by the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method by `entityTypeId` — the [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). The stage code for `crm.status.list` is assembled from this identifier.

**Semantics.** The semantics of a stage is the state of an item: in progress, successfully completed, or unsuccessfully completed. The method returns it in the `EXTRA.SEMANTICS` field: `process` for in progress, `success` for successfully completed, and `failure` and `apology` for unsuccessfully completed. In the top-level `SEMANTICS` field, final stages are marked with the `S` and `F` values, and for stages in progress it is empty. Semantics separates active items from closed ones even when stage names differ across pipelines.

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [How to Retrieve a List of Stages with Semantics for CRM Entities](./how-to-get-stages-with-semantics.md) | [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) | A table of CRM object stages with names and semantics ||
|| [How to Get Deal Pipelines with Stages and Semantics](./how-to-get-deal-funnels.md) | [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md), [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) | A table of stages with semantics for each deal pipeline ||
|| [How to Filter Items by Stage Name](./how-to-get-elements-by-stage-filter.md) | [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md), [crm.status.list](../../../api-reference/crm/status/crm-status-list.md), [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) | A list of CRM items at the selected stage ||
|#

## Collecting Activities by CRM Items

An activity is a record in the card timeline: a call, a meeting, an email, or a planned action. The [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method selects activities by the `OWNER_TYPE_ID` and `OWNER_ID` pair, so the identifiers of CRM items are retrieved first and then passed to the activity filter. The `OWNER_TYPE_ID` values are returned by the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method.

The person responsible for an activity is stored as a user identifier. The first and last names for a report are retrieved with the [user.get](../../../api-reference/user/user-get.md) method.

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [How to Retrieve a List of Activities from Deals](./get-activity-list-by-deals.md) | [user.current](../../../api-reference/user/user-current.md), [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md), [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md), [user.get](../../../api-reference/user/user-get.md) | A table of activities for an employee's deals with deadlines and responsible persons ||
|#

## Preparing Data for Inventory Management

A vendor is not a separate CRM object but a contact or a company in a system category: `CATALOG_CONTRACTOR_CONTACT` for a contact and `CATALOG_CONTRACTOR_COMPANY` for a company. The identifier of such a category is retrieved with the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method filtered by code, and the items are then selected by it.

The retrieved identifiers are passed to the inventory management method [catalog.documentcontractor.add](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md) to link the vendor with an inventory document.

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [How to Retrieve a List of Vendors](./how-to-get-contractors.md) | [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md), [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) | A list of contacts or companies marked as vendors ||
|#

## Continue Learning

- [{#T}](../how-to-add-crm-objects/index.md)
- [{#T}](../how-to-edit-crm-objects/index.md)
- [{#T}](../../../api-reference/crm/universal/index.md)
- [{#T}](../../../api-reference/crm/status/index.md)
- [{#T}](../../../api-reference/crm/data-types.md)
