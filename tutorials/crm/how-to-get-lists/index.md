# Retrieving Lists in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so the assistant can utilize the official REST documentation.

{% endnote %}

CRM lists are data selections needed for integrations: duplicates by phone and email, CRM activities related to deals, stages, funnels, addresses, vendors, and entities at a selected stage.

A scenario is a sequence of requests for a single task. It describes the order of method calls and provides a code example.

> Quick navigation: [all scenarios](#choose-tutorial)

## Interaction with CRM Objects

Scenarios are linked to clients, deals, SPAs, and inventory management.

- **Leads, contacts, and companies.** Duplicates are searched by phone and email, then data for [leads](../../../api-reference/crm/leads/index.md), [contacts](../../../api-reference/crm/contacts/index.md), and [companies](../../../api-reference/crm/companies/index.md) is retrieved. The client's address is stored in [requisites](../../../api-reference/crm/requisites/index.md).
- **Deals.** The list of activities is built by `ID` of deals. Stages and funnels help select deals for subsequent actions in CRM. Basic operations are performed using the methods [crm.deal.*](../../../api-reference/crm/deals/index.md).
- **SPAs.** Entities are filtered by stages through `entityTypeId` — the identifier for the [CRM object type](../../../api-reference/crm/data-types.md#object_type). Entities can be retrieved using universal methods [crm.item.*](../../../api-reference/crm/universal/index.md).
- **Vendors.** Vendors are obtained from contacts or companies by category. The retrieved identifiers are passed to the inventory management method [catalog.documentcontractor.add](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md).

## How to Get Started

1. Identify the CRM object: lead, contact, company, deal, or SPA.
2. Find the required scenario in the table [How to Choose a Scenario](#choose-tutorial).
3. Check what access permissions and scopes are specified in the selected scenario.
4. Execute the methods in the order described in the scenario.
5. Use the obtained identifiers in subsequent CRM or inventory management requests.

## How to Choose a Scenario {#choose-tutorial}

#| 
|| **If you need** | **Open** ||
|| Find duplicate leads, contacts, and companies by phone or email | [How to Find Duplicates in CRM by Phone and Email](./search-by-phone-and-email.md) ||
|| Gather activities related to a salesperson's deals and display responsible parties | [How to Get a List of Activities from Deals](./get-activity-list-by-deals.md) ||
|| Retrieve CRM stages and semantic values for analyzing the object's status | [How to Get a List of Stages with Semantics for CRM Objects](./how-to-get-stages-with-semantics.md) ||
|| Get deal funnels and stages within each funnel | [How to Get Deal Funnels with Semantics for Each Stage](./how-to-get-deal-funnels.md) ||
|| Retrieve client requisites and the associated address | [How to Get Client Address from CRM](./how-to-get-address.md) ||
|| Find vendors among contacts or companies | [How to Get a List of Vendors](./how-to-get-contractors.md) ||
|| Match stage names with identifiers and retrieve entities by filter | [How to Filter Entities by Stage Name](./how-to-get-elements-by-stage-filter.md) ||
|#