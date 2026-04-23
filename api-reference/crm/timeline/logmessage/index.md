# Log Record Journal: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The log record journal serves as a utility for secondary events in the timeline. In the interface, these records are presented with a lower visual priority: a subdued icon and less emphasis compared to key events.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## How to Work with the Log Record Journal

1. Add a record using the [crm.timeline.logmessage.add](./crm-timeline-logmessage-add.md) method.
2. Retrieve a record by its ID using the [crm.timeline.logmessage.get](./crm-timeline-logmessage-get.md) method.
3. Output a list of records for a CRM entity using the [crm.timeline.logmessage.list](./crm-timeline-logmessage-list.md) method.
4. Delete a record using the [crm.timeline.logmessage.delete](./crm-timeline-logmessage-delete.md) method if it is no longer needed.
5. Customize the appearance of the record through the [icon](./icons/index.md) and [logo](./logo/index.md) sections.

## Key Limitations of the Section

- The methods [crm.timeline.logmessage.get](./crm-timeline-logmessage-get.md) and [crm.timeline.logmessage.list](./crm-timeline-logmessage-list.md) only return records created by the [crm.timeline.logmessage.add](./crm-timeline-logmessage-add.md) method. System records cannot be retrieved using these methods.
- A record can only be deleted using the [crm.timeline.logmessage.delete](./crm-timeline-logmessage-delete.md) method in the context of the application that created it.

## Relationship with Other Objects

**CRM Entities.** A log record is created for a specific CRM entity through the `fields.entityTypeId` and `fields.entityId` fields of the [crm.timeline.logmessage.add](./crm-timeline-logmessage-add.md) method.

**Log Record Icons.** The set of icons can be managed using the [crm.timeline.icon.*](./icons/index.md) methods, and then the icon code is used in `fields.iconCode` when creating a record.

**Log Record Logos.** The set of logos can be managed using the [crm.timeline.logo.*](./logo/index.md) methods to maintain a consistent visual style for log records.

## How to Choose an Icon or Logo

- Use [crm.timeline.icon.*](./icons/index.md) when you need to quickly label the type of event in the log record via `fields.iconCode`.
- Use [crm.timeline.logo.*](./logo/index.md) when a separate set of branded images for log records is required.
- Before adding a record, check the available codes through [crm.timeline.icon.list](./icons/crm-timeline-icon-list.md) and [crm.timeline.logo.list](./logo/crm-timeline-logo-list.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Core

#| 
|| **Method** | **Description** ||
|| [crm.timeline.logmessage.add](./crm-timeline-logmessage-add.md) | Adds a new log record to the timeline ||
|| [crm.timeline.logmessage.get](./crm-timeline-logmessage-get.md) | Retrieves information about a log record ||
|| [crm.timeline.logmessage.list](./crm-timeline-logmessage-list.md) | Retrieves a list of all log records for a specific entity ||
|| [crm.timeline.logmessage.delete](./crm-timeline-logmessage-delete.md) | Deletes a log record ||
|#

### Icons

#| 
|| **Method** | **Description** ||
|| [crm.timeline.icon.add](./icons/crm-timeline-icon-add.md) | Adds a new icon ||
|| [crm.timeline.icon.get](./icons/crm-timeline-icon-get.md) | Retrieves information about an icon ||
|| [crm.timeline.icon.list](./icons/crm-timeline-icon-list.md) | Retrieves a list of all available icons ||
|| [crm.timeline.icon.delete](./icons/crm-timeline-icon-delete.md) | Deletes an icon ||
|#

### Logos

#| 
|| **Method** | **Description** ||
|| [crm.timeline.logo.add](./logo/crm-timeline-logo-add.md) | Adds a new logo ||
|| [crm.timeline.logo.get](./logo/crm-timeline-logo-get.md) | Retrieves information about a logo ||
|| [crm.timeline.logo.list](./logo/crm-timeline-logo-list.md) | Retrieves a list of all available logos ||
|| [crm.timeline.logo.delete](./logo/crm-timeline-logo-delete.md) | Deletes a logo ||
|#