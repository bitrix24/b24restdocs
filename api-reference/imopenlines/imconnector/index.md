# Open Line Connectors: Overview of Methods

Open line connectors are a tool for integrating external messengers and social networks with Bitrix24. Connectors allow you to:

- respond to customer messages from various channels in a unified interface.

- connect chatbots to automate communication with customers.

- analyze dialogue statistics to assess the effectiveness of communications.

> Quick navigation: [all methods](#all-methods) 
>
> User documentation: [Connect Open Channels](https://helpdesk.bitrix24.com/open/9392259/)

## Connection of Connectors with Other Objects

**Open Lines**. Collect messages from connectors, manage queues, and distribute messages among employees. Work with open lines should be done using the group of methods [imopenlines.*](../openlines/index.md).

**CRM**. Based on messages, leads and deals can be automatically created using the methods [crm.lead.add](../../crm/leads/crm-lead-add.md) and [crm.deal.add](../../crm/deals/crm-deal-add.md).

**User**. The user ID can be obtained using the methods [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md).

**Chat**. The correspondence with users is managed by the methods [im.chat.*](../../chats/index.md) and [im.message.*](../../chats/messages/index.md).

**Chatbots**. Setting up automatic responses and chatbots is done using the methods [imbot.*](../../chat-bots/index.md).

## How to Connect a Connector

1. Register the connector using the method [imconnector.register](imconnector-register.md).

2. Activate it using the method [imconnector.activate](imconnector-activate.md).

3. Set the connector data using the method [imconnector.connector.data.set](imconnector-connector-data-set.md).

4. Check the connector status using the method [imconnector.status](imconnector-status.md) and ensure it is ready for operation.

## How to Work with Messages

You can send a message using the method [imconnector.send.messages](./imconnector-send-messages.md).

Sent messages can be modified using the method [imconnector.update.messages](imconnector-update-messages.md). This method changes the parameters of the user, message, and chat.

Messages from open lines can be deleted using the method [imconnector.delete.messages](imconnector-delete-messages.md).

## How to Add a Widget to the Contact Center

To add a connector widget to the Contact Center, use the widget code [CONTACT_CENTER](../../widgets/contact-center.md). This code must be specified in the `PLACEMENT` parameter of the method [placement.bind](../../widgets/placement-bind.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The `imconnector.*` methods in the current version do not support operation through webhooks.

{% endnote %}

### Connector

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [imconnector.register](imconnector-register.md) | Register a connector ||
    || [imconnector.activate](imconnector-activate.md) | Activate a connector ||
    || [imconnector.deactivate](imconnector-deactivate.md) | Deactivate a connector  ||
    || [imconnector.status](imconnector-status.md) | Get the connector status ||
    || [imconnector.connector.data.set](imconnector-connector-data-set.md) | Change connector settings ||
    || [imconnector.list](imconnector-list.md) | Get a list of connectors ||
    || [imconnector.unregister](imconnector-unregister.md) | Unregister a connector ||
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
    || [imconnector.send.messages](imconnector-send-messages.md) | Send messages to Bitrix24 ||
    || [imconnector.update.messages](imconnector-update-messages.md) | Modify sent messages ||
    || [imconnector.delete.messages](imconnector-delete-messages.md) | Delete sent messages ||
    || [imconnector.send.status.delivery](imconnector-send-status-delivery.md) | Update status to `delivered` ||
    || [imconnector.send.status.reading](imconnector-send-status-reading.md) | Update status to `read` ||
    || [imconnector.chat.name.set](imconnector-chat-name-set.md) | Set a new chat name ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnImConnectorDialogStart](./events/on-im-connector-dialog-start.md) | When a dialog is created ||
    || [OnImConnectorDialogFinish](./events/on-im-connector-dialog-finish.md) | When a dialog is closed ||
    || [OnImConnectorMessageAdd](./events/index.md) | When a new message is added ||
    || [OnImConnectorMessageDelete](./events/on-im-connector-message-delete.md) | When messages are deleted ||
    || [OnImConnectorMessageUpdate](./events/on-im-connector-message-update.md) | When messages are modified ||
    |#

{% endlist %}

## Continue Learning

- [How to Create an Open Lines Connector for Online Chat on the Site](../../../tutorials/openlines/example-connector.md)