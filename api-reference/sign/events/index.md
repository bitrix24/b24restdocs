# Overview of Events in KEDO

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to receive notifications about changes in the statuses of documents and signing participants.

A detailed explanation of working with events is provided in the article [Concept and Benefits of Event Processing](../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events in the section through the [application](../../../settings/app-installation/index.md) and the method [event.bind](../../events/event-bind.md).

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Events in Bitrix24](../../events/test-handler.md).

## Availability of Servers for Sending and Receiving Events

{% include notitle [Availability of Servers for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`sign.b2e`](../../scopes/permissions.md)
>
> Who can subscribe: a user with access to KEDO

#| 
|| **Event** | **Triggered** ||
|| [OnSignB2eDocumentStatusChanged](./on-sign-b2e-document-status-changed.md) | When the status of a document changes ||
|| [OnSignB2eMemberStatusChanged](./on-sign-b2e-member-status-changed.md) | When the status of a signing participant changes ||
|#