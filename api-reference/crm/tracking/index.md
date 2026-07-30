# Sales Intelligence in CRM: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Sales Intelligence in CRM allows you to link leads, deals, contacts, companies, and estimates with their sources and acquisition paths.

The methods in this section help create and utilize Sales Intelligence traces in the Bitrix24 REST API.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [What Is Sales Intelligence](https://helpdesk.bitrix24.com/open/8807085/)

## Getting Started

1. Collect source data: UTM parameters, channel, customer path, or another external identifier
2. Create a trace using the [crm.tracking.trace.add](./crm-tracking-trace-add.md) method
3. Pass the trace identifier to the method for creating a lead, deal, contact, company, or SPA item
4. If the trace is no longer used, delete it using the [crm.tracking.trace.delete](./crm-tracking-trace-delete.md) method

## Connection with CRM Objects

**Leads and Deals.** A trace helps link a created CRM object to the source of the inquiry. To create objects, use the [crm.lead.add](../leads/crm-lead-add.md) and [crm.deal.add](../deals/crm-deal-add.md) methods.

**Contacts, Companies, and SPAs.** Analytics data can be passed when creating related customers and custom CRM objects via the corresponding addition methods.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: Any user

#|
|| **Method** | **Description** ||
|| [crm.tracking.trace.add](./crm-tracking-trace-add.md) | Creates a Sales Intelligence trace and returns its identifier ||
|| [crm.tracking.trace.delete](./crm-tracking-trace-delete.md) | Deletes a Sales Intelligence trace ||
|#

## Continue Learning

- [{#T}](../../../tutorials/crm/how-to-use-analitycs/info-to-analitics.md)
- [{#T}](../deals/crm-deal-add.md)
- [{#T}](../contacts/crm-contact-add.md)
