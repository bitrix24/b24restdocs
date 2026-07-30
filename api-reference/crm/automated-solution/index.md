# Automated Solutions: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Digital workspaces are a separate section for smart processes that are not tied to CRM. A workspace can consist of one or more processes. Each has its own cards, pipelines, Kanban stages, Automation rules, and other features.

In your account, you can find them in the section *Automation > Digital Workspaces > List of Digital Workspaces*.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Digital Workspaces](https://helpdesk.bitrix24.com/open/19160354/)

## Getting Started

1. Create a workspace using the [crm.automatedsolution.add](./crm-automated-solution-add.md) method
2. Change the name, sorting, or other parameters using the [crm.automatedsolution.update](./crm-automated-solution-update.md) method
3. Retrieve a workspace by identifier using the [crm.automatedsolution.get](./crm-automated-solution-get.md) method
4. Find workspaces by filter using the [crm.automatedsolution.list](./crm-automated-solution-list.md) method
5. If a workspace is no longer needed, delete it using the [crm.automatedsolution.delete](./crm-automated-solution-delete.md) method
6. To check available fields, use the [crm.automatedsolution.fields](./crm-automated-solution-fields.md) method

## Connection with Other Objects

**SPAs.** A workspace combines one or more SPAs. First, create a workspace, then use its identifier when configuring the SPA type.

**CRM.** A workspace is located within CRM, but its SPAs do not have to be linked to leads, deals, contacts, or companies.

{% note info "" %}

In the self-hosted version of Bitrix24, digital workspaces are available starting from CRM version [24.300.0](../../../settings/cloud-and-on-premise/on-premise/versions.md).

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [crm.automatedsolution.add](./crm-automated-solution-add.md) | Creates a digital workspace ||
|| [crm.automatedsolution.update](./crm-automated-solution-update.md) | Updates the digital workspace ||
|| [crm.automatedsolution.get](./crm-automated-solution-get.md) | Returns a digital workspace by identifier ||
|| [crm.automatedsolution.list](./crm-automated-solution-list.md) | Returns a list of digital workspaces ||
|| [crm.automatedsolution.delete](./crm-automated-solution-delete.md) | Deletes a digital workspace ||
|| [crm.automatedsolution.fields](./crm-automated-solution-fields.md) | Returns the description of digital workspace fields ||
|#
