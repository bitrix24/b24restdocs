# Automation Rule Platform: Overview of the Section

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Chatbots in Bitrix24 reply in chats, process messenger commands and events, and work with files. The bot platform `imbot.v2` is intended for new development; the deprecated `imbot.*` methods are kept for existing integrations.

> Quick navigation: [How to Choose a Section](#choose)

## Capabilities of Chatbots in Bitrix24

Chatbots can:

- send and modify messages in chats
- receive events and respond to user actions
- work with slash commands
- upload and download files
- manage group chats and participants
- utilize keyboards, attachments, and other interface elements

## Current API Version {#v2}

[Chatbots 2.0](./chat-bots-v2/index.md) is the main section for new development. It includes:

- the Automation Rule platform `imbot.v2` for registering and managing bots
- the `im.v2` methods for reading events and working with a chat on behalf of a user, without registering a bot
- new methods for working with chats, messages, files, commands, and the interface

For a new bot, use this section.

{% note info "" %}

If the integration is already running in production or uses new fields and methods, first check the [Change Log for API imbot.v2](./chat-bots-v2/change-log.md). The entries are listed from newest to oldest.

{% endnote %}

## How to Get Started

1. Choose an [Authorization](./chat-bots-v2/index.md#auth) method — a webhook or OAuth — and review the method [Response Format](./chat-bots-v2/index.md#response-format) in the Chatbots 2.0 overview
2. Follow the [Quick Start](./chat-bots-v2/quick-start.md) for a complete working example: registration, receiving an event, and replying in the chat
3. In your application, register a bot with the method [imbot.v2.Bot.register](./chat-bots-v2/imbot.v2/bots/bot-register.md) — it returns the bot ID
4. Send a message on behalf of the bot with the method [imbot.v2.Chat.Message.send](./chat-bots-v2/imbot.v2/messages/chat-message-send.md)
5. Check the request parameters for a webhook and OAuth and the token refresh procedure in the article [How to Call Chatbot 2.0 Methods and Refresh the Authorization Token](./send-command.md)

## Deprecated Methods {#outdated}

[Deprecated API](./outdated/index.md) contains documentation for the previous version of methods `imbot.*`, `imbot.chat.*`, `imbot.dialog.get`, `imbot.message.*`, `imbot.command.*`, and related events.

These methods are retained to support existing integrations. For new solutions, use [Chatbots 2.0](./chat-bots-v2/index.md).

## How to Choose a Section {#choose}

#| 
|| **If you need** | **Open the section** ||
|| To create a new chatbot, configure events, commands, messages, and files in the new architecture | [Chatbots 2.0](./chat-bots-v2/index.md) ||
|| To read messenger events and work with a chat on behalf of a user, without registering a bot | [Working with Chat im.v2](./chat-bots-v2/im.v2/index.md) ||
|| To support an existing integration on `imbot.*` | [Deprecated API](./outdated/index.md) ||
|| To migrate an existing bot from `imbot.*` to `imbot.v2` | [Migration from imbot to imbot.v2](./chat-bots-v2/migration.md) ||
|#

## Continue Your Exploration

- [Chatbots 2.0](./chat-bots-v2/index.md)
- [Deprecated API](./outdated/index.md)