# Actions with Records in the Timeline: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Up to 7 records can be pinned in a CRM item timeline. Pinned records will simultaneously appear under the new activity creation form and in their original chronological position within the timeline.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Timeline in CRM Item](https://helpdesk.bitrix24.com/open/25816403/)

## Getting Started

1. Retrieve the timeline record identifier `id`
2. Specify the CRM item to which the record is linked: `ownerTypeId` and `ownerId`
3. Pin the record using the [crm.timeline.item.pin](./crm-timeline-item-pin.md) method
4. If a record should no longer be pinned, use the [crm.timeline.item.unpin](./crm-timeline-item-unpin.md) method

## Connection with CRM Entities

**CRM item.** The methods verify that the timeline record is linked to the CRM item passed in `ownerTypeId` and `ownerId`. The User must have permissions to edit this CRM item.

**Comments.** Use the [crm.timeline.comment.add](../comments/crm-timeline-comment-add.md) or [crm.timeline.comment.list](../comments/crm-timeline-comment-list.md) method to retrieve the `id` of a timeline record.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a User with permission to edit the CRM item

#|
|| **Method** | **Description** ||
|| [crm.timeline.item.pin](./crm-timeline-item-pin.md) | Pins a record in the timeline ||
|| [crm.timeline.item.unpin](./crm-timeline-item-unpin.md) | Unpins a record in the timeline ||
|#
