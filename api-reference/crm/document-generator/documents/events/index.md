# Overview of Events When Working with Documents

Events allow applications to respond to changes in near real-time, providing notifications for the creation, updating, or deletion of documents in the [CRM Document Generator](../../index.md).

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick Navigation: [All Events](#all-events)

## What the Handler Receives

All three events pass the same set of fields in `data.FIELDS`:

- `ID` — identifier of the document. Use it to retrieve the document data with the method [crm.documentgenerator.document.get](../crm-document-generator-document-get.md)
- `ENTITY_TYPE_ID` — identifier of the CRM object type the document is linked to, for example `1` — lead
- `ENTITY_ID` — identifier of the CRM object itself

The document file and links to it are not passed in the event. To retrieve `pdfUrl`, `imageUrl`, or the public link, call [crm.documentgenerator.document.get](../crm-document-generator-document-get.md) with the received `ID`.

## How to Receive Events

You can subscribe to document events through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Event Processing in Bitrix24](../../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered By** ||
|| [onCrmDocumentGeneratorDocumentAdd](./on-crm-document-generator-document-add.md) | When a document is generated manually or by the methods [crm.documentgenerator.document.add](../crm-document-generator-document-add.md) and [crm.documentgenerator.document.upload](../crm-document-generator-document-upload.md) ||
|| [onCrmDocumentGeneratorDocumentUpdate](./on-crm-document-generator-document-update.md) | When a document is modified manually or by the method [crm.documentgenerator.document.update](../crm-document-generator-document-update.md) ||
|| [onCrmDocumentGeneratorDocumentDelete](./on-crm-document-generator-document-delete.md) | When a document is deleted manually or by the method [crm.documentgenerator.document.delete](../crm-document-generator-document-delete.md) ||
|#