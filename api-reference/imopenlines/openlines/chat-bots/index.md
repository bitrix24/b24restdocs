# Chatbots in Open Channels: Overview of Methods

Chatbots in open channels help automate communication with customers:

- automatically respond to customer messages,

- redirect dialogues to operators,

- collect customer data, such as names and emails.

> Quick navigation: [all methods](#all-methods)

## Connection of Chatbots with Other Objects

**User**. The methods [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) allow you to obtain the user ID.

**Chat**. The methods [imbot.message.*](../../../chat-bots/messages/index.md) help send, modify, and delete messages.

**Open Channels**. Chatbots can transfer conversations to another line using [imopenlines.bot.session.transfer](./imopenlines-bot-session-transfer.md). Working with open channels should be done using the group of methods [imopenlines.*](../index.md).

{% note tip "User Documentation" %}

- [Create and configure Open Channels](https://helpdesk.bitrix24.com/open/25385203/)

{% endnote %}

## How to Use Chatbots in Open Channels

1. Register the chatbot using the method [im.bot.register](../../../chat-bots/imbot-register.md).

2. Set up automatic responses and scenarios.

3. Connect the chatbot to open channels.

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md), [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [imopenlines.bot.session.finish](./imopenlines-bot-session-finish.md) | Ends the dialogue ||
|| [imopenlines.bot.session.message.send](./imopenlines-bot-session-message-send.md) | Sends a welcome message ||
|| [imopenlines.bot.session.operator](./imopenlines-bot-session-operator.md) | Switches the dialogue to a free operator ||
|| [imopenlines.bot.session.transfer](./imopenlines-bot-session-transfer.md) | Transfers the dialogue to an operator by ID ||
|#

## Continue Learning

- [Example of Creating a Chatbot for Open Channels](../../../../tutorials/chat-bots/open-lines-bot.md)