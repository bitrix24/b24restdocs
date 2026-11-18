# Actions with Records in the Timeline: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `crm.timeline.item.*` methods pin and unpin records in a CRM item timeline. Up to seven records can be pinned in a single item. Pinned records appear both under the activity creation form and in their chronological position in the timeline. After a record is unpinned, it stays in its chronological position.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Timeline in CRM Item](https://helpdesk.bitrix24.com/open/25816403/)

## Getting Started

1. Retrieve the timeline record identifier `id`. The methods that return it are listed in [Relationship with Other Objects](#links)
2. Specify the CRM item to which the record is linked in `ownerTypeId` and `ownerId`
3. Pin the record using the [crm.timeline.item.pin](./crm-timeline-item-pin.md) method
4. Unpin the record using the [crm.timeline.item.unpin](./crm-timeline-item-unpin.md) method if it should no longer be pinned or you need to free up a slot for a new one

## Relationship with Other Objects {#links}

**CRM item.** A record is pinned in the timeline of a specific CRM item. Pass the [object type](../../data-types.md#object_type) in `ownerTypeId` and the item ID in `ownerId`. The record must be linked to this item.

**Timeline records.** The methods accept a timeline record identifier in `id`. This is not an activity ID. The following REST methods return this `id`:

- comments: [crm.timeline.comment.add](../comments/crm-timeline-comment-add.md) and [crm.timeline.comment.list](../comments/crm-timeline-comment-list.md)
- log records: [crm.timeline.logmessage.add](../logmessage/crm-timeline-logmessage-add.md) and [crm.timeline.logmessage.list](../logmessage/crm-timeline-logmessage-list.md)

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a User with permission to edit the CRM item

#|
|| **Method** | **Description** ||
|| [crm.timeline.item.pin](./crm-timeline-item-pin.md) | Pins a record in the timeline ||
|| [crm.timeline.item.unpin](./crm-timeline-item-unpin.md) | Unpins a record in the timeline ||
|#
