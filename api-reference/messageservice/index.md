# Message Providers: Overview of Methods

Message providers are services for integration with Bitrix24. They allow sending messages to clients. Communication channels can include SMS and other systems that identify the recipient by phone number.

> Quick navigation: [all methods](#all-methods)

## Connection of Providers with Other Objects

**CRM**. A message can be sent from the CRM detail form.

**Workflows**. A message can be sent automatically from a workflow action or Automation rule in CRM. The provider is specified in the workflow settings only through the Bitrix24 interface.

{% note tip "User Documentation" %}

- [Twilio integration: Create an account and connect it to Bitrix24](https://helpdesk.bitrix24.com/open/21991204/)

{% endnote %}

## Message Delivery Status

The unique message identifier `message_id` allows you to call the method [messageservice.message.status.update](./messageservice-message-status-update.md). This method sets the delivery status for a message sent via the provider.

## Overview of Methods {#all-methods}

> Scope: [`messageservice`](../scopes/permissions)
>
> Who can perform the method: administrator

#|
|| **Method** | **Description** ||
|| [messageservice.sender.add](./messageservice-sender-add.md) | Registers an SMS provider or message provider ||
|| [messageservice.sender.delete](./messageservice-sender-delete.md) | Deletes an SMS provider or message provider ||
|| [messageservice.sender.list](./messageservice-sender-list.md) | Retrieves a list of SMS providers or message providers ||
|| [messageservice.sender.update](./messageservice-sender-update.md) | Updates an SMS provider or message provider ||
|| [messageservice.message.status.update](./messageservice-message-status-update.md) | Updates the delivery status of a message ||
|#

## Continue Learning

-  [{#T}](./tutorial.md)