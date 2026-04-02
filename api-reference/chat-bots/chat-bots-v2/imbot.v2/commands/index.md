# Commands: Overview of Methods

This section contains methods for registering slash commands for a chat bot, modifying them, deleting them, and sending responses to command calls.

> Quick Navigation: [All Methods](#all-methods)

## How Slash Commands Work

Slash commands allow you to invoke bot functions in two ways:

- by typing `/command` in the chat input field,
- by triggering the command through a [keyboard button](../messages/message-keyboards.md).

In the second case, the command is used as a hidden action of the button and does not require manual text input. After the command is registered, the bot receives a call event and can respond via [imbot.v2.Command.answer](./command-answer.md).

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

- [Chatbots 2.0: Overview of Methods](../../index.md)
- [imbot.v2 Events](../events/index.md)