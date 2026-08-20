# Open Channel Chats and CRM: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Open channel chats store conversations with customers from online chat, messengers, and social networks. The `imopenlines.crm.chat.*` methods help find a chat by CRM object, get the last active chat, and manage participants.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Use chats in Open Channels](https://helpdesk.bitrix24.com/open/25967455/)

## Linking Chats with Other Objects

**CRM.** A chat can be linked to one of four CRM objects: [lead](../../../crm/leads/index.md), [deal](../../../crm/deals/index.md), [contact](../../../crm/contacts/index.md), or [company](../../../crm/companies/index.md). The object type and its identifier are passed to the [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) method.

**User.** An employee can be added to a chat by `USER_ID`. The user ID can be obtained using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods.

**Chatbot.** A bot can be added to the chat. Chatbot actions in open channels are performed by the [imopenlines.bot.*](../chat-bots/index.md) method group.

{% note info "" %}

To add a bot to an open channel chat, it must have the scope [crm](../../../scopes/permissions.md).

{% endnote %}

**Dialogs.** The chat history can be retrieved by the chat ID `CHAT_ID` using the [imopenlines.session.history.get](../sessions/imopenlines-session-history-get.md) method.

**Open Channels.** Add, modify, and delete open channels using the [imopenlines.*](../index.md) methods.

**Connector.** Chats are created through a connector. The communication channel can be an online chat, messenger, or social network. To connect a connector or change settings, use the [imconnector.*](../../imconnector/index.md) method group.

## How to Work with Chats

1. Get the list of chats for the CRM object using the [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) method
2. Find the last active chat using the [imopenlines.crm.chat.getLastId](./imopenlines-crm-chat-get-last-id.md) method
3. Add or remove participants using the [imopenlines.crm.chat.user.add](./imopenlines-crm-chat-user-add.md) and [imopenlines.crm.chat.user.delete](./imopenlines-crm-chat-user-delete.md) methods
4. Send a message using the [imopenlines.crm.message.add](../messages/imopenlines-crm-message-add.md) method
5. Pass `CHAT_ID` to [imopenlines.crm.lead.create](../sessions/imopenlines-crm-lead-create.md) to create a lead based on the dialog

{% note tip "User Documentation" %}

- [How to create and configure an open channel](https://helpdesk.bitrix24.com/open/25385203/)

- [Contact Center](https://helpdesk.bitrix24.com/open/24095446/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) | Retrieves chats for the CRM object ||
|| [imopenlines.crm.chat.getLastId](./imopenlines-crm-chat-get-last-id.md) | Retrieves the ID of the last chat for the CRM object ||
|| [imopenlines.crm.chat.user.add](./imopenlines-crm-chat-user-add.md) | Adds a user to an existing chat of the CRM object ||
|| [imopenlines.crm.chat.user.delete](./imopenlines-crm-chat-user-delete.md) | Removes a user from the chat of the CRM object ||
|#
