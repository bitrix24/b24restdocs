# Open Channel Chats and CRM: Overview of Methods

Open channel chats are a space within Bitrix24 for communicating with clients through external systems: online chat on the site, messengers, and social networks. They allow linking dialogues to CRM cards, adding users or chatbots to the dialogues.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to work with chats in Open Channels](https://helpdesk.bitrix24.com/open/25761311/)

## Linking Chats with Other Objects

**CRM**. Each chat is linked to one of four CRM objects: [leads](../../../crm/leads/index.md), [deals](../../../crm/deals/index.md), [contacts](../../../crm/contacts/index.md), or [companies](../../../crm/companies/index.md).

**User**. A user can be added to the chat. The user ID can be obtained using the methods [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md).

**Chatbot**. A bot can be added to the chat. Work with bots should be done using the methods [imopenlines.bot.*](../../../imopenlines/openlines/chat-bots/index.md).

{% note info"" %}

To add a bot to an open channel chat, it must have the scope [crm](../../../scopes/permissions.md).

{% endnote %}

**Dialogues**. The chat history can be retrieved by the chat ID `CHAT_ID` using the method [imopenlines.session.history.get](../../../imopenlines/openlines/sessions/imopenlines-session-history-get.md).

**Open Channels**. Adding, modifying, and deleting open channels is facilitated by the methods [imopenlines.*](../../../imopenlines/openlines/index.md).

**Connector**. Chats are created through a connector. The communication channel can be an online chat, messenger, or social network. To connect the connector or change settings, use the group of methods [imconnector.*](../../../imopenlines/imconnector/index.md).

## How to Work with Chats

1. Use [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) to get a list of chats linked to the CRM object.

2. Find the last active chat using [imopenlines.crm.chat.getLastId](./imopenlines-crm-chat-get-last-id.md).

3. Add or remove chat participants using the methods [imopenlines.crm.chat.user.add](./imopenlines-crm-chat-user-add.md) and [imopenlines.crm.chat.user.delete](./imopenlines-crm-chat-user-delete.md).

4. To send a message, use the method [imopenlines.crm.message.add](../../../imopenlines/openlines/messages/imopenlines-crm-message-add.md).

5. In [imopenlines.crm.lead.create](../../../imopenlines/openlines/sessions/imopenlines-crm-lead-create.md), pass the chat ID `CHAT_ID` and create a lead based on the dialogue.

{% note tip "User Documentation" %}

- [How to create and configure an open channel](https://helpdesk.bitrix24.com/open/25385203/)

- [Contact Center](https://helpdesk.bitrix24.com/open/24095446/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [imopenlines.crm.chat.getLastId](./imopenlines-crm-chat-get-last-id.md) | Retrieves the ID of the last chat ||
|| [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) | Retrieves the chat for the CRM object ||
|| [imopenlines.crm.chat.user.add](./imopenlines-crm-chat-user-add.md) | Adds a user to an existing chat ||
|| [imopenlines.crm.chat.user.delete](./imopenlines-crm-chat-user-delete.md) | Removes a user from the chat ||
|#