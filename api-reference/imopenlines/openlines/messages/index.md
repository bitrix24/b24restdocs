# Open Channel Messages: Overview of Methods

Messages in open channel chats can be sent by employees and chatbots. Sent messages can be saved as quick reply templates and reused. Each open channel chat is linked to a CRM entity.

> Quick navigation: [all methods](#all-methods)

## Linking Messages to Other Entities

**CRM.** A chat message is tied to one of four CRM entities: [lead](../../../crm/leads/index.md), [deal](../../../crm/deals/index.md), [contact](../../../crm/contacts/index.md), or [company](../../../crm/companies/index.md).

**User.** An employee sends a message in the open channel chat. The employee's ID can be obtained using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods.

**Chatbot.** A message can be sent on behalf of a chatbot. Work with bots should be done using the [imbot.*](../../../imopenlines/openlines/chat-bots/index.md) methods.

**Dialogs.** The chat history can be retrieved using the chat ID `CHAT_ID` with the [imopenlines.session.history.get](../sessions/imopenlines-session-history-get.md) method.

**Open Channels.** To add, modify, and delete channels, use the [imopenlines.*](../index.md) methods.

**Universal Lists.** The ID of the quick replies list `QUICK_ANSWERS_IBLOCK_ID` can be obtained using the [imopenlines.config.get](../imopenlines-config-get.md) method. Specify the list when creating a channel using the [imopenlines.config.add](../imopenlines-config-add.md) method, and when editing, use the [imopenlines.config.update](../imopenlines-config-update.md) method.

{% note tip "User Documentation" %}

- [Canned responses in Open Channel chats](https://helpdesk.bitrix24.com/open/25760371/)

- [Create and configure Open Channels](https://helpdesk.bitrix24.com/open/25385203/)

- [Contact Center](https://helpdesk.bitrix24.com/open/24095446/)

{% endnote %}

## How to Send a Message from CRM

1. Find the chat linked to the CRM entity. Use the [imopenlines.crm.chat.get](../chats/imopenlines-crm-chat-get.md) or [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) method.
2. Obtain the ID of the employee or chatbot using the [user.get](../../../user/user-get.md), [user.search](../../../user/user-search.md), or [imbot.bot.list](../../../chat-bots/imbot-bot-list.md) methods.
3. Pass the data to the [imopenlines.crm.message.add](./imopenlines-crm-message-add.md) method to send the message.

## How to Save a Quick Reply

1. Retrieve the chat history using the [imopenlines.session.history.get](../sessions/imopenlines-session-history-get.md) method and select the message to save.
2. Pass the IDs `CHAT_ID` and `MESSAGE_ID` to the [imopenlines.message.quick.save](./imopenlines-message-quick-save.md) method. The message will be saved in the quick replies list.

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can perform the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [imopenlines.crm.message.add](./imopenlines-crm-message-add.md) | Sends a message in the chat linked to the CRM entity ||
|| [imopenlines.message.quick.save](./imopenlines-message-quick-save.md) | Saves a message as a quick reply ||
|#