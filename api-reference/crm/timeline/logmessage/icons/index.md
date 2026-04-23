# Log Record Icons: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Log record icons help visually distinguish entries in the CRM timeline.

Using the methods in this section, you can add a custom icon, retrieve data by code, display a list of available icons, and delete an icon.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Considerations Before Calling Methods

- The methods [crm.timeline.icon.add](./crm-timeline-icon-add.md) and [crm.timeline.icon.delete](./crm-timeline-icon-delete.md) can only be managed by an administrator.
- The methods [crm.timeline.icon.get](./crm-timeline-icon-get.md) and [crm.timeline.icon.list](./crm-timeline-icon-list.md) are available to any user.
- To create an icon, pass `fileContent` in `base64`. Use a `PNG` file sized `24x24` pixels with a transparent background.
- In the responses of the methods [crm.timeline.icon.get](./crm-timeline-icon-get.md) and [crm.timeline.icon.list](./crm-timeline-icon-list.md), the `isSystem` field indicates the type of icon: `true` for system icons and `false` for custom icons.

## How to Work with Icons

1. Retrieve the list of available codes using [crm.timeline.icon.list](./crm-timeline-icon-list.md).
2. Add a new icon using the method [crm.timeline.icon.add](./crm-timeline-icon-add.md).
3. Check the icon by code using the method [crm.timeline.icon.get](./crm-timeline-icon-get.md).
4. Delete the custom icon using the method [crm.timeline.icon.delete](./crm-timeline-icon-delete.md) if it is no longer in use.

## Relation to Other Objects

**Log Record.** The icon code is passed in the `fields.iconCode` field of the method [crm.timeline.logmessage.add](../crm-timeline-logmessage-add.md). 

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [crm.timeline.icon.add](./crm-timeline-icon-add.md) | Adds a new icon ||
|| [crm.timeline.icon.get](./crm-timeline-icon-get.md) | Retrieves information about an icon ||
|| [crm.timeline.icon.list](./crm-timeline-icon-list.md) | Retrieves a list of all available icons ||
|| [crm.timeline.icon.delete](./crm-timeline-icon-delete.md) | Deletes an icon ||
|#