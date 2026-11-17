# Overview of Events When Working with Documents

Events allow applications to respond to changes in near real-time, providing notifications for the creation, updating, or deletion of documents in the [CRM Document Generator](../../index.md).

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick Navigation: [All Events](#all-events)

## What the Handler Receives

All three events pass the same set of fields in `data.FIELDS`:

- `ID` — identifier of the document
- `ENTITY_TYPE_ID` — identifier of the CRM object type the document is linked to, for example `1` — lead
- `ENTITY_ID` — identifier of the CRM object itself

The document file and links to it are not passed in the event. For a created or updated document, you can retrieve its data, including available file links, by calling [crm.documentgenerator.document.get](../crm-document-generator-document-get.md) with the `ID`. The call requires permission to view generator documents and access to the document. Subscribing to the event does not grant these permissions. After a successful deletion, the document cannot be retrieved; retain any data you need beforehand.

Example fragment of a document creation event body in JSON format:

```json
{
    "event": "ONCRMDOCUMENTGENERATORDOCUMENTADD",
    "data": {
        "FIELDS": {
            "ID": "1042",
            "ENTITY_TYPE_ID": "2",
            "ENTITY_ID": "123"
        }
    }
}
```

The full request structure is described on the individual event pages below.

## How to Receive Events

You can subscribe to document events through:

- [outbound webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

Events are not sent to an application until its installation is complete. [Check the Application Installation](../../../../../settings/app-installation/installation-finish.md).

An example of a handler code for the event is described in the article [How to Test Your Handler for Event Processing in Bitrix24](../../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered By** ||
|| [onCrmDocumentGeneratorDocumentAdd](./on-crm-document-generator-document-add.md) | When a document is created manually or using [crm.documentgenerator.document.add](../crm-document-generator-document-add.md), or a ready-made document is uploaded using [crm.documentgenerator.document.upload](../crm-document-generator-document-upload.md) ||
|| [onCrmDocumentGeneratorDocumentUpdate](./on-crm-document-generator-document-update.md) | When a document is modified manually or by the method [crm.documentgenerator.document.update](../crm-document-generator-document-update.md) ||
|| [onCrmDocumentGeneratorDocumentDelete](./on-crm-document-generator-document-delete.md) | When deletion of a document is attempted manually or using [crm.documentgenerator.document.delete](../crm-document-generator-document-delete.md). The event is generated before deletion is complete ||
|#