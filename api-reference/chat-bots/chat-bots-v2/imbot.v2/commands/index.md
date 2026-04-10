# Commands: Overview of Methods

This section contains methods for registering, modifying, deleting, and responding to chat bot slash commands.

> Quick navigation: [all methods](#all-methods)

## How Slash Commands Work

Slash commands allow you to invoke bot functions in two ways:

- by typing `/command` in the chat input field,
- by triggering the command through a [keyboard button](../messages/message-keyboards.md).

In the second case, the command acts as a hidden action of the button and does not require manual text input. After the command is registered, the bot receives a call event and can respond via [imbot.v2.Command.answer](./command-answer.md).

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Command.register](./command-register.md) | Registers a slash command ||
|| [imbot.v2.Command.update](./command-update.md) | Updates a command ||
|| [imbot.v2.Command.list](./command-list.md) | Returns a list of bot commands ||
|| [imbot.v2.Command.unregister](./command-unregister.md) | Deletes a command ||
|| [imbot.v2.Command.answer](./command-answer.md) | Responds to a command call ||
|#

## Continue Your Exploration

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](../../index.md)
- [imbot.v2 Events](../events/index.md)