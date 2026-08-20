# Chatbots in Open Channels: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Chatbots in open channels help automate dialog processing. A bot can send a message to a customer, switch a dialog to a free operator, transfer a request to a specific employee, or finish a session.

> Quick Navigation: [All Methods](#all-methods)
>
> User documentation: [Create and configure Open Channels](https://helpdesk.bitrix24.com/open/25385203/)

## Connection of Chatbots with Other Entities

**User.** The employee identifier `USER_ID` is needed to transfer a dialog to a specific operator. It can be obtained using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods.

**Chat.** A bot works inside an open channel chat. For regular chatbot scenarios outside open channels, use the [imbot.*](../../../chat-bots/outdated/index.md) methods.

**Open Channels.** Chatbots use the current open channel session. Transfer a conversation to another line or employee using [imopenlines.bot.session.transfer](./imopenlines-bot-session-transfer.md), or finish a session using [imopenlines.bot.session.finish](./imopenlines-bot-session-finish.md).

{% note tip "User Documentation" %}

- [Create and configure Open Channels](https://helpdesk.bitrix24.com/open/25385203/)

{% endnote %}

## How to Use Chatbots in Open Channels

1. Register the chatbot using the [im.bot.register](../../../chat-bots/outdated/bots/imbot-register.md) method
2. Connect the chatbot to an open channel in the line settings
3. Send messages and manage dialogs using `imopenlines.bot.session.*` methods

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md), [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [imopenlines.bot.session.message.send](./imopenlines-bot-session-message-send.md) | Sends an automatic message in the dialogue ||
|| [imopenlines.bot.session.operator](./imopenlines-bot-session-operator.md) | Switches the dialogue to a free operator ||
|| [imopenlines.bot.session.transfer](./imopenlines-bot-session-transfer.md) | Transfers the dialogue to an operator by ID or queue ||
|| [imopenlines.bot.session.finish](./imopenlines-bot-session-finish.md) | Ends the dialogue ||
|#

## Continue Your Learning

- [How to Create a Chatbot for Open Channels](../../../../tutorials/chat-bots/open-lines-bot.md)
