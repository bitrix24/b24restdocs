# Automation Rule Platform: Overview of the Section

This section consolidates the documentation for the chatbot API in Bitrix24. It includes both the current methods of the new Automation Rule platform `imbot.v2` and the deprecated methods from the previous generation `imbot.*`.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

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
- the agent mode `im.v2`, which allows processing user events in the messenger
- new methods for working with chats, messages, files, commands, and the interface

If you are starting to create a new Chatbot, focus on this section.

{% note info "" %}

If the integration is already running in production or uses new fields and methods, first check the [Change Log for API imbot.v2](./chat-bots-v2/change-log.md). The entries are listed from newest to oldest.

{% endnote %}

## Deprecated Methods {#outdated}

[Deprecated API](./outdated/index.md) contains documentation for the previous version of methods `imbot.*`, `imbot.chat.*`, `imbot.message.*`, `imbot.command.*`, and related events.

These methods are retained to support existing integrations. For new solutions, it is strongly recommended to use [Chatbots 2.0](./chat-bots-v2/index.md).

## How to Choose a Section

#| 
|| **If you need** | **Open the section** ||
|| To create a new chatbot, configure events, commands, messages, and files in the new architecture | [Chatbots 2.0](./chat-bots-v2/index.md) ||
|| To support an existing integration on `imbot.*` | [Deprecated API](./outdated/index.md) ||
|| To migrate an existing bot from `imbot.*` to `imbot.v2` | [Migration from imbot to imbot.v2](./chat-bots-v2/migration.md) ||
|#

## Continue Your Exploration

- [Chatbots 2.0](./chat-bots-v2/index.md)
- [Deprecated API](./outdated/index.md)