# Overview of Events When Working with Documents

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of documents.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to document events through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`documentgenerator, crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [onCrmDocumentGeneratorDocumentAdd](./on-crm-document-generator-document-add.md) | When a document is generated manually or by the method [crm.documentgenerator.document.add](../crm-document-generator-document-add.md) ||
|| [onCrmDocumentGeneratorDocumentUpdate](./on-crm-document-generator-document-update.md) | When a document is modified or by the method [crm.documentgenerator.document.update](../crm-document-generator-document-update.md) ||
|| [onCrmDocumentGeneratorDocumentDelete](./on-crm-document-generator-document-delete.md) | When a document is deleted or by the method [crm.documentgenerator.document.delete](../crm-document-generator-document-delete.md) ||
|#