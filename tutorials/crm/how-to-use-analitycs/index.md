# Sales Intelligence in CRM: Common Scenarios

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Sales Intelligence helps you understand which source a customer came from and what they did before reaching out: clicked an ad, viewed website pages, filled out a form, or called. In the CRM, this data can be linked to a lead, deal, contact, company, or quotation.

For example, a customer visits a website from an advertising campaign, views several pages, and submits a form. If you pass the Sales Intelligence data to the CRM, the manager will see the source of the inquiry, and reports will show which ad brought in the customer and which deal it is linked to.

Tutorials demonstrate how to pass analytics data when creating CRM objects or how to link existing objects to a single trace. Select a practical scenario from the table below.

> Quick links: [All Scenarios](#choose-tutorial)

## Connection with CRM Objects

Sales Intelligence data can be passed to five CRM objects: lead, contact, company, deal, and quotation. There are two methods:

- **When creating an object** — along with its fields in the creation method. The source of the inquiry is passed via UTM fields, and the full customer journey is passed via the `TRACE` field, if the creation method supports this field. For the universal method [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md), the full trace is linked using a separate method [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md). See the [tutorial](./info-to-analitics.md) for which fields are available for each object.
- **By linking existing objects** — using the [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method. The object types — `LEAD`, `CONTACT`, `COMPANY`, `DEAL`, `QUOTE` — and their IDs are passed in the `ENTITIES` parameter. This allows you to link a single object or multiple objects to one trace.

## Requirements

**Scope.** All scenarios require the [`crm`](../../../api-reference/scopes/permissions.md) scope.

**Permissions.** To create a CRM object, the user must have permission to add an object of the corresponding type. To link a trace to a created or existing object, the user must have permission to update that object. If a deal is linked to a contact, permission to read the contact is also required.

The exact permissions for each scenario are specified at the top of its page.

## Getting Started

1. Select a scenario from the [How to Choose a Scenario](#choose-tutorial) table.
2. Prepare a form on your website to collect customer data.
3. If the full customer journey is required, retrieve `TRACE` via `b24Tracker.guest.getTrace()`. Pass it when creating the object if the method supports the field `TRACE`, or link the object to a trace via [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md). For objects created with the universal method [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md), use [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md). If the source of the inquiry is sufficient, pass the UTM fields.
4. Check the permissions and scopes specified in the selected scenario.
5. Execute the methods in the order described in the scenario.

## How to Choose a Scenario {#choose-tutorial}

#|
|| **If You Need** | **Open** | **Result** ||
|| Pass Sales Intelligence when creating a lead from a form | [How to Use Sales Intelligence When Creating a Lead](./use-analitics-for-add-lead.md) | The lead is created and linked to the inquiry source and visitor trace ||
|| Create a contact and a deal, then link them to a single trace | [How to Use Sales Intelligence When Creating a Deal and Contact](./use-analitics-for-add-contact.md) | The contact and associated deal are created and linked to the same trace ||
|| Select a data transfer method: UTM fields, `TRACE`, or a separate trace | [How to Pass Data to CRM Sales Intelligence](./info-to-analitics.md) | A method for passing the inquiry source or complete customer journey is selected ||
|| View the CRM tracking method reference | [Sales Intelligence in CRM: Overview of Methods](../../../api-reference/crm/tracking/index.md) | Methods for adding and deleting traces are selected ||
|#

## Continue Learning

- [{#T}](../../../api-reference/crm/tracking/index.md)
- [{#T}](../../../api-reference/crm/universal/index.md)
- [{#T}](../how-to-add-crm-objects/index.md)
