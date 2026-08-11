# Commands: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods manage the slash commands of the chatbot: registration, updating, deletion, and sending a response to the command call.

> Quick navigation: [all methods](#all-methods)

## How Slash Commands Work

Slash commands allow you to invoke bot functions in two ways:

- enter `/command` in the chat input field
- call the command via the [keyboard button](../messages/message-keyboards.md)

In the second case, the command is used as a hidden action of the button and does not require manual text input.

## How to Get Started {#how-to-start}

1. Register the command using the [imbot.v2.Command.register](./command-register.md) method. The title and the parameter description are set by a `{langCode: text}` object — this is what the user sees in the list of commands.
2. Wait for the [ONIMBOTV2COMMANDADD](../events/events.md#onimbotv2commandadd) event. It delivers `command.id`, `message.id`, and `chat.dialogId`.
3. Reply to the user with the [imbot.v2.Command.answer](./command-answer.md) method, passing `commandId`, `messageId`, and `dialogId` from the event. The bot can reply even in a chat it does not belong to: access is granted temporarily for the `messageId` + `commandId` pair.
4. If necessary, modify the command with [imbot.v2.Command.update](./command-update.md) or delete it with [imbot.v2.Command.unregister](./command-unregister.md).

To check which commands are already registered for the bot, use the [imbot.v2.Command.list](./command-list.md) method.

## Where the Command Is Available {#scope}

#|
|| **Value of `fields.common`** | **Where the Command Is Available** | **When to Use It** ||
|| `true` | In any chat, even if the bot is not a participant | Global actions: help, search ||
|| `false` — the default value | Only in a private dialog with the bot and in chats where the bot is a participant | Actions tied to a specific bot and the context of its chat ||
|#

Hidden commands with `fields.hidden: true` are not shown in the list of available commands but remain callable — for example, from a keyboard button.

## Command Call Context {#context}

The `command.context` field in the [ONIMBOTV2COMMANDADD](../events/events.md#onimbotv2commandadd) event shows where the command came from.

#|
|| **Value** | **What Happened** ||
|| `textarea` | The user entered the command manually in the input field ||
|| `keyboard` | The user tapped a keyboard button ||
|| `menu` | The user selected the command from the context menu ||
|#

If a single message contains several slash commands, a separate event is generated for each command.

## Relationship with Other Objects {#relations}

**Bot.** A command is registered for a specific bot: `botId` is passed in all calls, and for webhook authorization, `botToken` as well — [Bots](../bots/index.md).

**Events.** A command call reaches the bot as the [ONIMBOTV2COMMANDADD](../events/events.md#onimbotv2commandadd) event in the delivery mode set for the bot — [Events](../events/index.md).

**Keyboards.** A command invoked by a button arrives in the event with the value `command.context = keyboard` — the bot uses it to distinguish a tap from manual input. Button configuration is described in [Working with Keyboards](../messages/message-keyboards.md).

**Messages.** A response to a command is a regular bot message: the `fields` parameter of the [imbot.v2.Command.answer](./command-answer.md) method accepts text of up to 20,000 characters, attachments, and a keyboard — [Messages](../messages/index.md).

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
- [{#T}](../../migration.md)
- [imbot.v2 Events](../events/index.md)
- [Messages imbot.v2](../messages/index.md)
- [{#T}](../messages/message-keyboards.md)