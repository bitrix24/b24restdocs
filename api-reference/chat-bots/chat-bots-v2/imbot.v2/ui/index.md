# Chat Interface: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section contains methods for managing chat interface elements on behalf of the bot: the action indicator and the availability of the input field.

> Quick Navigation: [All Methods](#all-methods)

## What Can Be Changed in the Interface

- Show the user the bot's action indicator
- Temporarily enable or disable the text input field

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of a registered bot

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Chat.InputAction.notify](./chat-input-action-notify.md) | Displays the bot's action indicator ||
|| [imbot.v2.Chat.TextField.enabled](./chat-text-field-enabled.md) | Enables or disables the text input field ||
|#

## Continue Your Exploration

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](../../index.md)
- [Messages imbot.v2](../messages/index.md)