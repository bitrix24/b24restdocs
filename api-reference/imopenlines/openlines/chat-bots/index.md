# Chatbots in Open Channels: Overview of Methods

Chatbots in Open Channels help automate communication with customers by:

- automatically responding to customer messages,
- redirecting conversations to operators,
- collecting customer data, such as names and emails.

> Quick Navigation: [All Methods](#all-methods)

## Connection of Chatbots with Other Entities

**User**. You can obtain the user ID using the methods [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md).

**Chat**. The methods [imbot.message.*](../../../chat-bots/outdated/messages/index.md) assist in sending, modifying, and deleting messages.

**Open Channels**. Chatbots can transfer conversations to another line using [imopenlines.bot.session.transfer](./imopenlines-bot-session-transfer.md). Work with Open Channels should be done using the group of methods [imopenlines.*](../index.md).

{% note tip "User Documentation" %}

- [Create and configure Open Channels](https://helpdesk.bitrix24.com/open/25385203/)

{% endnote %}

## How to Use Chatbots in Open Channels

1. Register the chatbot using the method [im.bot.register](../../../chat-bots/outdated/bots/imbot-register.md).
   
2. Set up automatic responses and scenarios.

3. Connect the chatbot to Open Channels.

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md), [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [imopenlines.bot.session.message.send](./imopenlines-bot-session-message-send.md) | Sends an automatic message in the dialogue ||
|| [imopenlines.bot.session.operator](./imopenlines-bot-session-operator.md) | Switches the dialogue to a free operator ||
|| [imopenlines.bot.session.transfer](./imopenlines-bot-session-transfer.md) | Transfers the dialogue to an operator by ID or queue ||
|| [imopenlines.bot.session.finish](./imopenlines-bot-session-finish.md) | Ends the dialogue ||
|#

## Continue Your Learning

- [Example of Creating a Chatbot for Open Channels](../../../../tutorials/chat-bots/open-lines-bot.md)