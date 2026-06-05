# Commands: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods manage the slash commands of the chatbot: registration, updating, deletion, and sending a response to the command call.

> Quick navigation: [all methods](#all-methods)

## How Slash Commands Work

Slash commands allow you to invoke bot functions in two ways:

- enter `/command` in the chat input field,
- call the command via the [keyboard button](../messages/message-keyboards.md).

In the second case, the command is used as a hidden action of the button and does not require manual text input. After registering the command, the bot receives a call event and can respond through [imbot.v2.Command.answer](./command-answer.md).

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Command.register](./command-register.md) | Registers a slash command ||
|| [imbot.v2.Command.update](./command-update.md) | Updates a command ||
|| [imbot.v2.Command.list](./command-list.md) | Returns a list of the bot's commands ||
|| [imbot.v2.Command.unregister](./command-unregister.md) | Deletes a command ||
|| [imbot.v2.Command.answer](./command-answer.md) | Responds to a command call ||
|#

## Continue Your Exploration

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](../../index.md)
- [imbot.v2 Events](../events/index.md)