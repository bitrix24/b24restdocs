# Overview of Events in CRM Activities

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in near real-time, providing notifications about the creation, updating, and deletion of activities in the CRM timeline.

A detailed explanation of working with events is provided in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to activity events through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the [event.bind](../../../../events/event-bind.md) method

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../../events/test-handler.md).

## What the Handler Receives

Activity events pass only the identifier of the activity in the `data.FIELDS.ID` field. To retrieve the remaining data, call the [crm.activity.get](../activity-base/crm-activity-get.md) method.

The events are triggered for activities of all kinds: system, universal, configurable, and activities of custom types.

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered** ||
|| [onCrmActivityAdd](./on-crm-activity-add.md) | When an activity is created manually or via the [crm.activity.add](../activity-base/crm-activity-add.md) method ||
|| [onCrmActivityUpdate](./on-crm-activity-update.md) | When an activity is updated manually or via the [crm.activity.update](../activity-base/crm-activity-update.md) method ||
|| [onCrmActivityDelete](./on-crm-activity-delete.md) | When an activity is deleted manually or via the [crm.activity.delete](../activity-base/crm-activity-delete.md) method ||
|#