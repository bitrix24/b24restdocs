# Open Channels Connectors: Overview of Methods and Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Open Channels connectors link an external communication channel to Bitrix24. Through a connector, the application registers a channel, sends customer messages to an open channel, and receives events about messages, dialogues, and line disconnection.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Communication channels in Contact Center](https://helpdesk.bitrix24.com/open/25935795/)

## Connection of Connectors with Other Objects

**Open Channels.** A line receives messages from the connector, applies queue settings, and distributes dialogues between employees. Settings and sessions are managed by the [imopenlines.*](../openlines/index.md) method group.

**CRM.** Open Channels dialogues can be linked to leads, deals, contacts, and companies. The [imopenlines.crm.lead.create](../openlines/sessions/imopenlines-crm-lead-create.md) method can create a lead from a dialogue.

**User.** The employee identifier `USER_ID` is needed for routing and adding participants to the chat. It can be obtained using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods.

**Chat.** Correspondence between a customer and an employee is stored in the Open Channel chat. The `chat.id` and `im.chat_id` identifiers link the external chat to the Bitrix24 chat in methods for sending, updating, and deleting messages.

**Chatbots.** Bots can reply in a dialogue, transfer a conversation to an employee, and close a session. Use the [imopenlines.bot.*](../openlines/chat-bots/index.md) method group for bot actions in an open channel.

## How to Connect a Connector

1. Register the connector using the [imconnector.register](imconnector-register.md) method
2. Activate the connector using the [imconnector.activate](imconnector-activate.md) method
3. Set the connector data using the [imconnector.connector.data.set](imconnector-connector-data-set.md) method
4. Check channel readiness using the [imconnector.status](imconnector-status.md) method

## How to Work with Messages

You can send a message using the method [imconnector.send.messages](./imconnector-send-messages.md).

Sent messages can be modified using the method [imconnector.update.messages](imconnector-update-messages.md). This method updates message, user, and chat data in the external channel.

Messages in open channels can be deleted using the method [imconnector.delete.messages](imconnector-delete-messages.md).

## How to Add a Widget to the Contact Center

To add a connector widget to the Contact Center, use the widget code [CONTACT_CENTER](../../widgets/contact-center.md). This code must be specified in the `PLACEMENT` parameter of the method [placement.bind](../../widgets/placement-bind.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`imconnector`](../../scopes/permissions.md), [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute methods: any user

{% note info "" %}

The methods `imconnector.*` in the current version do not support operation via webhooks.

{% endnote %}

### Connector

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [imconnector.register](imconnector-register.md) | Registers a connector ||
    || [imconnector.activate](imconnector-activate.md) | Activates a connector ||
    || [imconnector.status](imconnector-status.md) | Retrieves the connector status ||
    || [imconnector.connector.data.set](imconnector-connector-data-set.md) | Changes connector settings ||
    || [imconnector.list](imconnector-list.md) | Retrieves a list of connectors ||
    || [imconnector.unregister](imconnector-unregister.md) | Unregisters a connector ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [OnImConnectorLineDelete](./events/on-im-connector-line-delete.md) | When an open line is deleted ||
    || [OnImConnectorStatusDelete](./events/on-im-connector-status-delete.md) | When an open line is disabled ||
    |#

{% endlist %}

### Chats and Messages

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [imconnector.send.messages](imconnector-send-messages.md) | Sends external channel messages to Bitrix24 ||
    || [imconnector.update.messages](imconnector-update-messages.md) | Modifies sent messages ||
    || [imconnector.delete.messages](imconnector-delete-messages.md) | Deletes sent messages ||
    || [imconnector.send.status.delivery](imconnector-send-status-delivery.md) | Updates the `delivered` status ||
    || [imconnector.chat.name.set](imconnector-chat-name-set.md) | Sets a new chat name ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [OnImConnectorDialogStart](./events/on-im-connector-dialog-start.md) | When a dialog is created ||
    || [OnImConnectorDialogFinish](./events/on-im-connector-dialog-finish.md) | When a dialog is closed ||
    || [OnImConnectorMessageAdd](./events/on-im-connector-message-add.md) | When a new message is added ||
    || [OnImConnectorMessageDelete](./events/on-im-connector-message-delete.md) | When messages are deleted ||
    || [OnImConnectorMessageUpdate](./events/on-im-connector-message-update.md) | When messages are updated ||
    |#

{% endlist %}

## Continue Learning

- [How to Create an Open Channels Connector for Website Chat](../../../tutorials/openlines/example-connector.md)
