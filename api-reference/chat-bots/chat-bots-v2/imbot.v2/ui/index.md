# Chat Interface: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods control the chat interface elements on behalf of the bot: the action indicator and the availability of the input field.

> Quick navigation: [all methods](#all-methods)

## What Can Be Changed in the Interface

- show the user the bot's action indicator
- temporarily enable or disable the text input field

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Chat.InputAction.notify](./chat-input-action-notify.md) | Displays the bot's action indicator ||
|| [imbot.v2.Chat.TextField.enabled](./chat-text-field-enabled.md) | Enables or disables the text input field ||
|#

## Continue Your Exploration

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](../../index.md)
- [Messages for imbot.v2](../messages/index.md)