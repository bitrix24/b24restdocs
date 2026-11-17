# Overview of Events When Working with Comments

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in near real-time: receiving notifications about the addition, update, or deletion of timeline comments.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to timeline comment events through:

- [outbound webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the [event.bind](../../../../events/event-bind.md) method

For example, to subscribe to comment creation in an application, call `BX24.callMethod('event.bind', {event: 'onCrmTimelineCommentAdd', handler: 'https://www.my-domain.com/handler/'});`.

An example of an event handler is provided in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../../events/test-handler.md).

## What the Handler Receives

The event passes the comment identifier in the `data.FIELDS.ID` field. To retrieve the data of a created or updated comment, call the [crm.timeline.comment.get](../crm-timeline-comment-get.md) method. The user must have read permission for at least one linked CRM item.

Once a comment is deleted, its data can no longer be retrieved with this method.

Event specifics for updating a comment with multiple bindings are described in the [crm.timeline.comment.update](../crm-timeline-comment-update.md) method.

## Availability of Servers for Sending and Receiving Events

{% include notitle [Availability of Servers for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered** ||
|| [onCrmTimelineCommentAdd](./on-Crm-Timeline-Comment-Add.md) | When a new comment is created in the timeline manually or via the [crm.timeline.comment.add](../crm-timeline-comment-add.md) method ||
|| [onCrmTimelineCommentUpdate](./on-Crm-Timeline-Comment-Update.md) | When a comment in the timeline is updated manually or via the [crm.timeline.comment.update](../crm-timeline-comment-update.md) method ||
|| [onCrmTimelineCommentDelete](./on-Crm-Timeline-Comment-Delete.md) | When a comment in the timeline is deleted manually or via the [crm.timeline.comment.delete](../crm-timeline-comment-delete.md) method ||
|#