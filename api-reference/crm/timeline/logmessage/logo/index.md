# Logotypes for Log Records: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Logotypes for log records help visually distinguish entries in the CRM timeline.

Using the methods in this section, you can add a custom logotype, retrieve data by code, display a list of available logotypes, and delete a logotype.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Considerations Before Calling Methods

- The methods [crm.timeline.logo.add](./crm-timeline-logo-add.md) and [crm.timeline.logo.delete](./crm-timeline-logo-delete.md) can only be managed by an administrator.
- The methods [crm.timeline.logo.get](./crm-timeline-logo-get.md) and [crm.timeline.logo.list](./crm-timeline-logo-list.md) are available to any user.
- To create a logotype, pass `fileContent` in `base64`. Use a `PNG` file sized `60x60` pixels with a transparent background.

## How to Work with Logotypes

1. Retrieve the list of available codes through [crm.timeline.logo.list](./crm-timeline-logo-list.md).
2. Add a new logotype using the method [crm.timeline.logo.add](./crm-timeline-logo-add.md).
3. Check the logotype by code using the method [crm.timeline.logo.get](./crm-timeline-logo-get.md).
4. Delete the custom logotype using the method [crm.timeline.logo.delete](./crm-timeline-logo-delete.md) if it is no longer in use.

## Relation to Other Objects

**Log Record Journal.** Logotypes are related to the [Log Record Journal](../index.md), which contains methods for creating, reading, and deleting log records.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [crm.timeline.logo.add](./crm-timeline-logo-add.md) | Adds a new logotype ||
|| [crm.timeline.logo.get](./crm-timeline-logo-get.md) | Retrieves information about a logotype ||
|| [crm.timeline.logo.list](./crm-timeline-logo-list.md) | Retrieves a list of all available logotypes ||
|| [crm.timeline.logo.delete](./crm-timeline-logo-delete.md) | Deletes a logotype ||
|#