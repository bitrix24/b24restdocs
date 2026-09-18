# Overview of Events When Working with Companies

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of [companies](../index.md).

In all three events, the handler receives only the company ID in `data.FIELDS.ID`, without field values. After creation and update, retrieve the company data with the method [crm.item.get](../../universal/crm-item-get.md) with `entityTypeId = 4` or the deprecated method [crm.company.get](../crm-company-get.md). After deletion, the company is no longer available, so store any data you may need beforehand.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to company events through:

- [outbound webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The methods `crm.company.add`, `crm.company.update`, and `crm.company.delete` are deprecated, but the events are still triggered when they are called. For new integrations, use the universal methods `crm.item.*` with `entityTypeId = 4`.

#|
|| **Event** | **Triggered** ||
|| [onCrmCompanyAdd](./on-crm-company-add.md) | When a company is created manually, via the method [crm.company.add](../crm-company-add.md), or via the method [crm.item.add](../../universal/crm-item-add.md) with `entityTypeId = 4` ||
|| [onCrmCompanyUpdate](./on-crm-company-update.md) | When a company is updated manually, via the method [crm.company.update](../crm-company-update.md), or via the method [crm.item.update](../../universal/crm-item-update.md) with `entityTypeId = 4` ||
|| [onCrmCompanyDelete](./on-crm-company-delete.md) | When a company is deleted manually, via the method [crm.company.delete](../crm-company-delete.md), or via the method [crm.item.delete](../../universal/crm-item-delete.md) with `entityTypeId = 4` ||
|#
