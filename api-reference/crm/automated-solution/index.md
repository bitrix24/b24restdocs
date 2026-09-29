# Automated Solutions: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Digital workspaces are a separate section for smart processes that do not have to be linked to leads, deals, contacts, or companies. A workspace can consist of one or more processes. Each has its own cards, pipelines, Kanban stages, Automation rules, and other features.

In Bitrix24, they are located in the section *Automation > Digital Workspaces > List of Digital Workspaces*.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Digital Workspaces](https://helpdesk.bitrix24.com/open/19160354/)

## Getting Started

1. Retrieve the `id` of an SPA type using the [crm.type.list](../universal/user-defined-object-types/crm-type-list.md) method, or create a type using the [crm.type.add](../universal/user-defined-object-types/crm-type-add.md) method
2. Create a workspace using the [crm.automatedsolution.add](./crm-automated-solution-add.md) method and pass the type `id` values in the `typeIds` field
3. Check the result using the [crm.automatedsolution.get](./crm-automated-solution-get.md) method, or find workspaces by filter using the [crm.automatedsolution.list](./crm-automated-solution-list.md) method
4. Change the name or the set of SPAs using the [crm.automatedsolution.update](./crm-automated-solution-update.md) method
5. Delete the workspace using the [crm.automatedsolution.delete](./crm-automated-solution-delete.md) method. Before deleting, remove its SPAs: pass an empty `typeIds` to `crm.automatedsolution.update`, and the SPAs return to CRM

## Connection with Other Objects

**SPAs.** SPAs are bound on the workspace side, in the `typeIds` field. Pass the SPA type identifiers (`id`) from the [Smart Processes](../universal/user-defined-object-types/index.md) section to `typeIds`, not `entityTypeId`. The `customSectionId` and `customSections` parameters of the `crm.type.*` methods are deprecated and are not used to configure workspaces.

**CRM.** Workspaces and their SPAs run on CRM and are managed by methods with the `crm` scope, but they are displayed in the *Automation* section. An SPA can belong to only one place: CRM or a single workspace. When bound to a workspace, it is removed from its previous place.

## Limitations

- The number of digital workspaces is limited. When the limit is exceeded, the `crm.automatedsolution.add` method returns the `LIMIT_EXCEEDED` error
- To move an SPA from CRM to a workspace or return it to CRM, the "User can edit preferences" CRM permission is required in addition to permissions for digital workspaces

{% note info "" %}

In the self-hosted version of Bitrix24, digital workspaces are available starting from CRM version [24.300.0](../../../settings/cloud-and-on-premise/on-premise/versions.md).

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the methods: a Bitrix24 administrator or a user with the "Edit automation solutions" or "Edit settings" permission for "Automated solutions" in CRM access permissions

#|
|| **Method** | **Description** ||
|| [crm.automatedsolution.add](./crm-automated-solution-add.md) | Creates a digital workspace ||
|| [crm.automatedsolution.update](./crm-automated-solution-update.md) | Updates the digital workspace ||
|| [crm.automatedsolution.get](./crm-automated-solution-get.md) | Returns a digital workspace by identifier ||
|| [crm.automatedsolution.list](./crm-automated-solution-list.md) | Returns a list of digital workspaces ||
|| [crm.automatedsolution.delete](./crm-automated-solution-delete.md) | Deletes a digital workspace ||
|| [crm.automatedsolution.fields](./crm-automated-solution-fields.md) | Returns the description of digital workspace fields ||
|#
