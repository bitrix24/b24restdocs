# Open Channels Messages: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Bitrix24 can link an open channel chat to a lead, deal, contact, or company. In such a chat, an application writes to the customer on behalf of an employee or a chatbot — for example, on behalf of the manager responsible for the deal. A well-phrased operator reply can be saved as a quick reply and reused.

> Quick Navigation: [All Methods](#all-methods)
>
> User documentation: [Canned responses in Open Channel chats](https://helpdesk.bitrix24.com/open/25760371/)

## Linking Messages to Other Entities

**CRM.** A message is sent to a chat linked to a [lead](../../../crm/leads/index.md), [deal](../../../crm/deals/index.md), [contact](../../../crm/contacts/index.md), or [company](../../../crm/companies/index.md). The object type and its ID are passed in the `CRM_ENTITY_TYPE` and `CRM_ENTITY` parameters of the [imopenlines.crm.message.add](./imopenlines-crm-message-add.md) method. How a chat is linked to a CRM object is described in the [open channel chats](../chats/index.md) overview.

**User.** The ID of the employee on whose behalf the message is sent is passed in `USER_ID`. You can find an employee using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods.

**Chatbot.** A message can also be sent on behalf of a chatbot: in this case, pass the bot ID in `USER_ID`. Other bot actions in a dialog are covered by the [imopenlines.bot.*](../chat-bots/index.md) method group.

**Conversation History.** It is returned by the [imopenlines.session.history.get](../sessions/imopenlines-session-history-get.md) method — pass `CHAT_ID` to it.

**Open Channels.** To add, modify, or delete channels, use the [imopenlines.config.*](../index.md) methods.

**Universal Lists.** The ID of the quick replies list `QUICK_ANSWERS_IBLOCK_ID` can be obtained using the [imopenlines.config.get](../imopenlines-config-get.md) method. You can specify the list when creating a channel using the [imopenlines.config.add](../imopenlines-config-add.md) method, and when editing, use the [imopenlines.config.update](../imopenlines-config-update.md) method.

{% note tip "User Documentation" %}

- [Create and configure Open Channels](https://helpdesk.bitrix24.com/open/25385203/)
- [Contact Center](https://helpdesk.bitrix24.com/open/24095446/)

{% endnote %}

## How to Send a Message from CRM

1. Find the chat linked to the CRM object using the [imopenlines.crm.chat.get](../chats/imopenlines-crm-chat-get.md) method without `ACTIVE_ONLY`. The method returns chats in which the dialog is still ongoing — a message can be sent only to them
2. Retrieve the author ID: for an employee, use the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods; for a chatbot, use the [imbot.v2.Bot.list](../../../chat-bots/chat-bots-v2/imbot.v2/bots/bot-list.md) method. The author must be a chat participant. If the author is not in the chat, add them using the [imopenlines.crm.chat.user.add](../chats/imopenlines-crm-chat-user-add.md) method
3. Send the message using the [imopenlines.crm.message.add](./imopenlines-crm-message-add.md) method: pass the CRM object, the chat, the author, and the text. The method returns the ID of the new message. Both the author and the user calling the method need read permission for the CRM object

## How to Save a Quick Reply

1. Retrieve the chat history using the [imopenlines.session.history.get](../sessions/imopenlines-session-history-get.md) method and select the message you want to save
2. Pass the IDs `CHAT_ID` and `MESSAGE_ID` to the [imopenlines.message.quick.save](./imopenlines-message-quick-save.md) method. The message will be saved in the quick replies list of this chat's channel

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method — for [imopenlines.crm.message.add](./imopenlines-crm-message-add.md), both the caller and the message author need read access to the CRM object the chat is linked to; for [imopenlines.message.quick.save](./imopenlines-message-quick-save.md) — a member of the open channel chat or a user with the "Join conversation" permission for this channel

#|
|| **Method** | **Description** ||
|| [imopenlines.crm.message.add](./imopenlines-crm-message-add.md) | Sends a message in the chat linked to the CRM object ||
|| [imopenlines.message.quick.save](./imopenlines-message-quick-save.md) | Saves a message as a quick reply ||
|#
