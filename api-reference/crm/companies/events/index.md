# Overview of Events When Working with Companies

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of companies.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events) 

## How to Receive Events

You can subscribe to company events through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmCompanyAdd](./on-crm-company-add.md) | When a company is created manually or via the method [crm.company.add](../crm-company-add.md) ||
|| [onCrmCompanyUpdate](./on-crm-company-update.md) | When a company is updated manually or via the method [crm.company.update](../crm-company-update.md) ||
|| [onCrmCompanyDelete](./on-crm-company-delete.md) | When a company is deleted manually or via the method [crm.company.delete](../crm-company-delete.md) ||
|#