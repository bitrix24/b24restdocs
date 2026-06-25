# Message Providers: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Message providers are services for integration with Bitrix24. They allow you to send messages to customers. Communication channels can include SMS and other systems that identify the recipient by phone number.

> Quick navigation: [All Methods](#all-methods)

## Connection with Other Objects

**CRM**. A message can be sent from a CRM card.

**Workflows**. A message can be sent automatically from a workflow action or a CRM automation rule. The provider is specified in the workflow configurations only via the Bitrix24 interface.

{% note tip "User Documentation" %}

- [Twilio integration: Create an account and connect it to Bitrix24](https://helpdesk.bitrix24.com/open/21991204/)

{% endnote %}

## Message Delivery Status

The message Unique ID `message_id` allows you to call the [messageservice.message.status.update](./messageservice-message-status-update.md) method. The method sets the delivery status for a message sent using a provider.

## Overview of Methods {#all-methods}

> Scope: [`messageservice`](../scopes/permissions)
>
> Who can execute the method: administrator

#|
|| **Method** | **Description** ||
|| [messageservice.sender.add](./messageservice-sender-add.md) | Registers an SMS provider or message provider ||
|| [messageservice.sender.delete](./messageservice-sender-delete.md) | Deletes an SMS provider or message provider ||
|| [messageservice.sender.list](./messageservice-sender-list.md) | Retrieves a list of SMS providers or message providers ||
|| [messageservice.sender.update](./messageservice-sender-update.md) | Updates an SMS provider or message provider ||
|| [messageservice.message.status.update](./messageservice-message-status-update.md) | Updates the delivery status of a message ||
|#

## Continue Learning

-  [{#T}](../../tutorials/messageservice/index.md)