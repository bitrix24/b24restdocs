# How to Use Sales Intelligence Data

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Sales Intelligence helps you understand which source a customer came from and what they did before reaching out: clicked an ad, viewed website pages, filled out a form, or called. In the CRM, this data can be linked to a lead, deal, contact, company, or quotation.

For example, a customer visits a website from an advertising campaign, views several pages, and submits a form. If you pass the Sales Intelligence data to the CRM, the manager will see the source of the inquiry, and reports will show which ad brought in the customer and which deal it is linked to.

Tutorials demonstrate how to pass analytics data when creating CRM objects or how to link existing objects to a single trace. Select a practical scenario from the table below.

> Quick links: [All Scenarios](#choose-tutorial)

## Connection with CRM Objects

Sales Intelligence data can be passed to five CRM objects: lead, contact, company, deal, and quotation. There are two methods:

- **When creating an object** — along with its fields in the creation method. The source of the inquiry is passed via UTM fields, and the full customer journey is passed via the `TRACE` field, if the creation method supports this field. For the universal method [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md), the full trace is linked using a separate method [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md). See the [tutorial](./info-to-analitics.md) for which fields are available for each object.
- **By linking existing objects** — using the [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method. The object types — `LEAD`, `CONTACT`, `COMPANY`, `DEAL`, `QUOTE` — and their IDs are passed in the `ENTITIES` parameter. This allows you to link a single object or multiple objects to one trace.

## Getting Started

1. Select a scenario from the [How to Choose a Scenario](#choose-tutorial) table.
2. Prepare a form on your website to collect customer data.
3. If the full customer journey is required, retrieve `TRACE` via `b24Tracker.guest.getTrace()`. Pass it when creating the object if the method supports the field `TRACE`, or link the object to a trace via [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md). For objects created with the universal method [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md), use [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md). If the source of the inquiry is sufficient, pass the UTM fields.
4. Check the permissions and scopes specified in the selected scenario.
5. Execute the methods in the order described in the scenario.

## How to Choose a Scenario {#choose-tutorial}

#|
|| **If needed** | **Open** ||
|| Pass Sales Intelligence when creating a lead from a form | [How to use Sales Intelligence when creating a lead](./use-analitics-for-add-lead.md) ||
|| Create a contact and a deal, then link them to a single trace | [How to use Sales Intelligence when creating a deal and a contact](./use-analitics-for-add-contact.md) ||
|| Select a data transfer method: UTM fields, `TRACE` or a separate trace | [How to pass data to Sales Intelligence CRM](./info-to-analitics.md) ||
|| View the CRM tracking methods directory | [Sales Intelligence in CRM: methods overview](../../../api-reference/crm/tracking/index.md) ||
|#
