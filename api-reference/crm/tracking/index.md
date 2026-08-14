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

1. Collect source data in the `TRACE` JSON string: retrieve it on the website via `b24Tracker.guest.getTrace()` or create UTM parameters in the `tags.list` object
2. Create a new CRM object using the [crm.item.add](../universal/crm-item-add.md) method or retrieve the identifier of an existing object using the [crm.item.list](../universal/crm-item-list.md) method
3. Create a trace using the [crm.tracking.trace.add](./crm-tracking-trace-add.md) method and pass the link to the CRM object in the `ENTITIES` parameter
4. If the trace is no longer used, delete it using the [crm.tracking.trace.delete](./crm-tracking-trace-delete.md) method

{% note tip "Common Cases and Scenarios" %}

- [How to Pass Sales Intelligence Data to CRM](../../../tutorials/crm/how-to-use-analitycs/info-to-analitics.md)

{% endnote %}

## Connection with CRM Objects

**Leads, deals, contacts, companies, and estimates.** A trace helps link a CRM object to the source of the inquiry. In the `ENTITIES` parameter of the [crm.tracking.trace.add](./crm-tracking-trace-add.md) method, pass the object type in `TYPE` and its identifier in `ID`. You can retrieve the identifier using the [crm.item.list](../universal/crm-item-list.md) method or from the response of the [crm.item.add](../universal/crm-item-add.md) method.

**Object types.** For a lead, use `TYPE = LEAD`; for a deal, `DEAL`; for a contact, `CONTACT`; for a company, `COMPANY`; for an estimate, `QUOTE`. If you retrieve the identifier using [crm.item.list](../universal/crm-item-list.md), pass the `entityTypeId` of the required type: `1` for a lead, `2` for a deal, `3` for a contact, `4` for a company, `7` for an estimate.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: Any user

#|
|| **Method** | **Description** ||
|| [crm.tracking.trace.add](./crm-tracking-trace-add.md) | Creates a Sales Intelligence trace, can link it to CRM objects, and returns the trace identifier ||
|| [crm.tracking.trace.delete](./crm-tracking-trace-delete.md) | Deletes a Sales Intelligence trace by the identifier from the add method response ||
|#

## Continue Learning

- [{#T}](../universal/crm-item-add.md)
- [{#T}](../universal/crm-item-list.md)
