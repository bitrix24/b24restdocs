# How to use Sales Intelligence data

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Sales Intelligence helps you understand which source a client came from and what they did before reaching out: clicked on an ad, opened website pages, filled out a form, or called. In the CRM, this data can be linked to a lead, deal, contact, company, or estimate.

For example, a client visits a website from an advertising campaign, views several pages, and submits a form. If you pass the Sales Intelligence data to the CRM, the manager will see the source of the request, and reports will show which advertisement brought in the client and which deal it is linked to.

Tutorials demonstrate how to pass analytics data when creating CRM objects or how to link existing objects to a single trace. Choose a practical scenario from the table below.

> Quick links: [all scenarios](#choose-tutorial)

## Linking to CRM objects

Sales Intelligence data can be passed to five CRM objects: lead, contact, company, deal, and estimate. There are two methods:

- **When creating an object** — along with its fields in the creation method. The request source is passed via UTM fields, and the full customer journey is passed via the `TRACE` field. For supported fields, see the [tutorial](./info-to-analitics.md)
- **By linking existing objects** — using the [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method. In the `ENTITIES` parameter, you pass the object types — `LEAD`, `CONTACT`, `COMPANY`, `DEAL`, `QUOTE` — and their identifiers. This allows you to link a single object or multiple objects to one trace.

## Getting Started

1. Select a scenario from the [How to choose a scenario](#choose-tutorial) table
2. Prepare a form on the website to collect client data
3. If the full customer journey is required, retrieve `TRACE` via `b24Tracker.guest.getTrace()` and pass it when creating an object or via [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) for linked objects. If the request source is sufficient, pass the UTM fields
4. Check which permissions and scopes are specified in the chosen scenario
5. Execute the methods in the order described in the scenario

## How to choose a scenario {#choose-tutorial}

#|
|| **If needed** | **Open** ||
|| Pass Sales Intelligence when creating a lead from a form | [How to use Sales Intelligence when creating a lead](./use-analitics-for-add-lead.md) ||
|| Create a contact and a deal, then link them to a single trace | [How to use Sales Intelligence when creating a deal and a contact](./use-analitics-for-add-contact.md) ||
|| Select a data transfer method: UTM fields, `TRACE`, or a separately linked trace | [How to pass data to Sales Intelligence](./info-to-analitics.md) ||
|| View the CRM tracking methods overview | [Sales Intelligence in CRM: methods overview](../../../api-reference/crm/tracking/index.md) ||
|#
