# Chat Interface: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods control the chat interface elements on behalf of the bot: the action indicator and the availability of the input field.

> Quick navigation: [all methods](#all-methods)

## When to Use the Methods of This Section {#when-to-use}

#|
|| **Task** | **Method** | **How It Looks to the User** ||
|| Show that the bot is working on a response while a long operation is running | [imbot.v2.Chat.InputAction.notify](./chat-input-action-notify.md) | An action indicator appears in the chat header — either the standard “typing” or one of the agent statuses, for example “Agent is searching for information...” ||
|| Guide the user through the steps of a scenario and prevent arbitrary text input | [imbot.v2.Chat.TextField.enabled](./chat-text-field-enabled.md) | The input field is disabled, and the user replies only with keyboard buttons ||
|#

The indicator text is selected by the `statusMessageCode` code — the list of available codes is provided on the [imbot.v2.Chat.InputAction.notify](./chat-input-action-notify.md#status-codes) page. If the code is not specified, the standard “typing” indicator is displayed.

## Limits {#limits}

#|
|| **Limit** | **Value** ||
|| Duration of the action indicator | 1–600 seconds. If `duration` is not specified, the server determines the value ||
|| Access to the methods of this section | Checked by the bot's membership in the chat: `ACCESS_DENIED` is returned if the bot is not a participant of the specified dialog ||
|| Chat types | Both methods work in group chats as well as in private chats with the bot ||
|#

The recipient is set by the `dialogId` parameter — [Format of dialogId](../../index.md#dialog-id).

## Relationship with Other Objects {#relations}

**Bot.** The methods are executed on behalf of a registered bot: `botId` is passed in the calls, and for webhook authorization, `botToken` as well — [Bots](../bots/index.md).

**Messages.** The action indicator is usually displayed between receiving an event and sending a response with the [imbot.v2.Chat.Message.send](../messages/chat-message-send.md) method — [Messages](../messages/index.md).

**Keyboards.** A disabled input field is usually combined with buttons: the user continues the scenario by tapping instead of typing — [Working with Keyboards](../messages/message-keyboards.md).

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
- [{#T}](../../migration.md)
- [Messages for imbot.v2](../messages/index.md)
- [{#T}](../messages/message-keyboards.md)