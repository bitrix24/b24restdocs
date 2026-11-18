# Message Providers: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Message providers are services for integration with Bitrix24. They allow you to send messages to customers. Communication channels can include SMS and other systems that identify the recipient by phone number.

{% note info "" %}

The methods in this section work only in the context of an [application](../../settings/app-installation/index.md). A provider is registered for a specific application, and the same application receives messages in its handler.

{% endnote %}

> Quick navigation: [All Methods](#all-methods)

## How to Get Started

1. Register a provider using the [messageservice.sender.add](./messageservice-sender-add.md) method. Provide the provider code `CODE`, the type `TYPE` with the value `SMS`, the name `NAME`, and the handler address `HANDLER`. Only an administrator can register, modify, delete, and list providers.
2. Receive the message in the handler. When an employee sends a message from a CRM card or from a workflow, Bitrix24 sends a POST request to the address from `HANDLER`. In PHP, the values are available in `$_POST`. After the request is parsed, the data of a message sent from a CRM card looks like this:

    ```json
    {
        "module_id": "crm",
        "bindings": [
            {
                "OWNER_TYPE_ID": 3,
                "OWNER_ID": 123
            }
        ],
        "type": "SMS",
        "code": "provider1",
        "message_id": "65575980fa531ac284c2ee68f81ebebd",
        "message_to": "+19990000000",
        "message_body": "Your confirmation code: 1234",
        "ts": 1742895600
    }
    ```

    To send the message, you need `message_to`, `message_body`, and `message_id`. Retain `message_id` so you can update the delivery status later. The full set of data, including the workflow fields and the `auth` object, is described on the page of the [messageservice.sender.add](./messageservice-sender-add.md#handler) method.

    Bitrix24 does not wait for the handler's response and does not read it. Right after the handler is called, the message receives the `sent` status. Report an error or delivery using the method from step 4.

3. Send the message on your service side.
4. Update the delivery status using the [messageservice.message.status.update](./messageservice-message-status-update.md) method. Provide the provider code `CODE`, the received `message_id` in the `MESSAGE_ID` parameter, and the new status `STATUS`. Only an administrator can update the status of a message sent by another user.

To check which providers are already registered by the application, use the [messageservice.sender.list](./messageservice-sender-list.md) method. To change the name, description, or handler address, use the [messageservice.sender.update](./messageservice-sender-update.md) method; to delete a provider, use the [messageservice.sender.delete](./messageservice-sender-delete.md) method.

## Method Specifics

- A provider belongs to the application that registered it. The methods see and modify only the providers of the current application, and the `CODE` must be unique within the application
- When the application is deleted or updated to a new version, Bitrix24 deletes all its providers. After an update, register the providers again, for example, in the [ONAPPUPDATE](../common/events/on-app-update.md) event handler
- Only the `SMS` type is supported
- The `HANDLER` address must start with `http://` or `https://`, and the domain must contain a dot. The method does not accept an address like `http://localhost/handler`
- If the application is paid and has not been paid for, Bitrix24 does not call the handler, and the message is not sent
- The `messageservice.message.status.update` method does not check the order of statuses. Pass the statuses in the order your service reports them

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

- [{#T}](../../tutorials/messageservice/index.md)