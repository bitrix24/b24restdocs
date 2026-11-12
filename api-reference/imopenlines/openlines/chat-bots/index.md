# Chatbots in Open Channels: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Chatbots in open channels help automate dialog processing. A bot can send a message to a customer, switch a dialog to a free operator, transfer a request to a specific employee, or finish a session.

> Quick Navigation: [All Methods](#all-methods)
>
> User documentation: [Create and configure Open Channels](https://helpdesk.bitrix24.com/open/25385203/)

## Connection of Chatbots with Other Entities

**User.** The employee identifier `USER_ID` is needed to transfer a dialog to a specific operator. It can be obtained using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods.

**Chat.** A bot works inside an open channel chat. All methods accept the chat ID `CHAT_ID`. The bot receives it from the [ONIMBOTV2MESSAGEADD](../../../chat-bots/chat-bots-v2/imbot.v2/events/events.md#onimbotv2messageadd) event, and it is also returned by the [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) and [imopenlines.crm.chat.get](../chats/imopenlines-crm-chat-get.md) methods. The bot itself is registered and configured using the [Chatbots 2.0](../../../chat-bots/chat-bots-v2/index.md) bot platform methods. The same methods work for a regular chatbot outside open channels. For a new bot, use Chatbots 2.0: the deprecated `imbot.*` methods are kept only for existing integrations, and the migration steps are described in the article [Migration from imbot to imbot.v2](../../../chat-bots/chat-bots-v2/migration.md).

**Open Channels.** Chatbots work with the current dialog in an open channel chat. Transfer a dialog to a specific operator or to the queue using [imopenlines.bot.session.transfer](./imopenlines-bot-session-transfer.md): to transfer it to the queue, pass the channel ID from the [imopenlines.config.list.get](../imopenlines-config-list-get.md) response. Finish a dialog using [imopenlines.bot.session.finish](./imopenlines-bot-session-finish.md).

**Application.** The `imopenlines.bot.session.transfer` and `imopenlines.bot.session.finish` methods find the chatbot by the application that registered it. When calling them via a webhook, pass the `CLIENT_ID` parameter — the same one used to register the bot — or `botToken` for Chatbots 2.0.

{% note tip "User Documentation" %}

- [Create and configure Open Channels](https://helpdesk.bitrix24.com/open/25385203/)

{% endnote %}

## How to Use Chatbots in Open Channels

1. Register a chatbot with open channel support using the [imbot.v2.Bot.register](../../../chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md) method: in the `fields` object, pass `isSupportOpenline: true` or the bot type `openline`
2. Connect the chatbot to an open channel in the line settings
3. Handle the [ONIMBOTV2MESSAGEADD](../../../chat-bots/chat-bots-v2/imbot.v2/events/events.md#onimbotv2messageadd) event: it provides the bot with the open channel chat ID required by the session methods
4. Send messages and manage dialogs using `imopenlines.bot.session.*` methods

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md), [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the methods: [imopenlines.bot.session.message.send](./imopenlines-bot-session-message-send.md) and [imopenlines.bot.session.operator](./imopenlines-bot-session-operator.md) — any user; [imopenlines.bot.session.transfer](./imopenlines-bot-session-transfer.md) and [imopenlines.bot.session.finish](./imopenlines-bot-session-finish.md) — an application user with a registered chatbot

#|
|| **Method** | **Description** ||
|| [imopenlines.bot.session.message.send](./imopenlines-bot-session-message-send.md) | Sends an automatic message in the dialogue ||
|| [imopenlines.bot.session.operator](./imopenlines-bot-session-operator.md) | Switches the dialogue to a free operator ||
|| [imopenlines.bot.session.transfer](./imopenlines-bot-session-transfer.md) | Transfers the dialog to an employee or to an open channel queue ||
|| [imopenlines.bot.session.finish](./imopenlines-bot-session-finish.md) | Ends the dialogue ||
|#

## Continue Your Learning

- [How to Create a Chatbot for Open Channels](../../../../tutorials/chat-bots/open-lines-bot.md)
- [Chatbots 2.0](../../../chat-bots/chat-bots-v2/index.md)
