# Overview of Events When Working with Comments

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in near real-time: receiving notifications about the addition, update, or deletion of timeline comments.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to timeline comment events through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the [event.bind](../../../../events/event-bind.md) method

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../../events/test-handler.md).

## Availability of Servers for Sending and Receiving Events

{% include notitle [Availability of Servers for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

## Events

#| 
|| **Event** | **Triggered** ||
|| [onCrmTimelineCommentAdd](./on-Crm-Timeline-Comment-Add.md) | When a new comment is created in the timeline manually or via the [crm.timeline.comment.add](../crm-timeline-comment-add.md) method ||
|| [onCrmTimelineCommentUpdate](./on-Crm-Timeline-Comment-Update.md) | When a comment in the timeline is updated manually or via the [crm.timeline.comment.update](../crm-timeline-comment-update.md) method ||
|| [onCrmTimelineCommentDelete](./on-Crm-Timeline-Comment-Delete.md) | When a comment in the timeline is deleted manually or via the [crm.timeline.comment.delete](../crm-timeline-comment-delete.md) method ||
|#