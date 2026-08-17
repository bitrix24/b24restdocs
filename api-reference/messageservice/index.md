# Message Providers: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Message providers are services for integration with Bitrix24. They allow you to send messages to customers. Communication channels can include SMS and other systems that identify the recipient by phone number.

{% note info "" %}

The methods in this section work only in the context of an [application](../../settings/app-installation/index.md). A provider is registered for a specific application, and the same application receives messages in its handler.

{% endnote %}

> Quick navigation: [All Methods](#all-methods)

## How to Get Started

1. Register a provider using the [messageservice.sender.add](./messageservice-sender-add.md) method. Provide the provider code `CODE`, the type `TYPE` with the value `SMS`, the name `NAME`, and the handler address `HANDLER`. Only an administrator can register, modify, and delete providers.
2. Wait for a message to be sent. When an employee sends a message from a CRM card or from a workflow, Bitrix24 calls the address from `HANDLER` and passes the message data: the recipient's number, the text, and the unique identifier `message_id`. The full set of data is described on the page of the [messageservice.sender.add](./messageservice-sender-add.md#handler) method.
3. Send the message on your service side.
4. Update the delivery status using the [messageservice.message.status.update](./messageservice-message-status-update.md) method. Provide the provider code `CODE`, the received `message_id` in the `MESSAGE_ID` parameter, and the new status `STATUS`. Only an administrator can update the status of a message sent by another user.

To check which providers are already registered by the application, use the [messageservice.sender.list](./messageservice-sender-list.md) method. To change the name, description, or handler address, use the [messageservice.sender.update](./messageservice-sender-update.md) method; to delete a provider, use the [messageservice.sender.delete](./messageservice-sender-delete.md) method.

## Connection with Other Objects

**CRM.** Messages are sent from a CRM card. The application handler receives the `module_id` parameter with the value `crm` and the `bindings` array with the message bindings to CRM objects.

**Workflows.** A message is sent automatically from a workflow action or a CRM automation rule. The provider is specified in the workflow configurations only via the Bitrix24 interface. The application handler receives the `module_id` parameter with the value `bizproc`, as well as the identifiers of the workflow and the document.

{% note tip "User Documentation" %}

- [Twilio integration: Create an account and connect it to Bitrix24](https://helpdesk.bitrix24.com/open/21991204/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [messageservice.sender.add](./messageservice-sender-add.md) | Registers an SMS provider or message provider ||
|| [messageservice.sender.update](./messageservice-sender-update.md) | Updates an SMS provider or message provider ||
|| [messageservice.sender.list](./messageservice-sender-list.md) | Retrieves a list of SMS providers or message providers ||
|| [messageservice.sender.delete](./messageservice-sender-delete.md) | Deletes an SMS provider or message provider ||
|| [messageservice.message.status.update](./messageservice-message-status-update.md) | Updates the delivery status of a message ||
|#

## Continue Learning

-  [{#T}](../../tutorials/messageservice/index.md)