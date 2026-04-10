---
title: Migration from imbot to imbot.v2
---

# Migration from imbot to imbot.v2

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A mapping table of methods and events between the deprecated API (imbot) and the new bot platform (imbot.v2).

{% note info "" %}

Methods v1 and v2 operate in parallel. Bots registered through v1 are visible in v2 and vice versa. However, the event formats differ — the bot receives events only in the format of the API version through which it was registered.

{% endnote %}

## Methods

#|
|| **v1** | **v2** | **Changes** ||
|| [imbot.register](../outdated/bots/imbot-register.md) | [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md) | Parameters are nested in `fields.*`, added `fields.eventMode` (fetch/webhook) ||
|| [imbot.update](../outdated/bots/imbot-update.md) | [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md) | Parameters are nested in `fields.*` ||
|| [imbot.unregister](../outdated/bots/imbot-unregister.md) | [imbot.v2.Bot.unregister](./imbot.v2/bots/bot-unregister.md) | No changes ||
|| [imbot.bot.list](../outdated/bots/imbot-bot-list.md) | [imbot.v2.Bot.list](./imbot.v2/bots/bot-list.md) | Returns an array of Bot objects instead of a flat list ||
|| — | [imbot.v2.Bot.get](./imbot.v2/bots/bot-get.md) | New method: retrieve a single bot by ID ||
|| [imbot.message.add](../outdated/messages/imbot-message-add.md) | [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md) | Text and parameters in `fields.*` ||
|| [imbot.message.update](../outdated/messages/imbot-message-update.md) | [imbot.v2.Chat.Message.update](./imbot.v2/messages/chat-message-update.md) | Parameters in `fields.*` ||
|| [imbot.message.delete](../outdated/messages/imbot-message-delete.md) | [imbot.v2.Chat.Message.delete](./imbot.v2/messages/chat-message-delete.md) | No changes ||
|| — | [imbot.v2.Chat.Message.read](./imbot.v2/messages/chat-message-read.md) | New method: mark messages as read ||
|| — | [imbot.v2.Chat.Message.Reaction.add](./imbot.v2/messages/chat-message-reaction-add.md) | New method: add a reaction ||
|| — | [imbot.v2.Chat.Message.Reaction.delete](./imbot.v2/messages/chat-message-reaction-delete.md) | New method: remove a reaction ||
|| [imbot.message.like](../outdated/messages/imbot-message-like.md) | [imbot.v2.Chat.Message.Reaction.add](./imbot.v2/messages/chat-message-reaction-add.md), [imbot.v2.Chat.Message.Reaction.delete](./imbot.v2/messages/chat-message-reaction-delete.md) | In v2, setting and removing a reaction are separated into two methods ||
|| [imbot.chat.add](../outdated/chats/imbot-chat-add.md) | [imbot.v2.Chat.add](./imbot.v2/chats/chat-add.md) | Parameters in `fields.*` ||
|| [imbot.chat.get](../outdated/chats/imbot-chat-get.md) | [imbot.v2.Chat.get](./imbot.v2/chats/chat-get.md) | Returns a Chat object ||
|| [imbot.dialog.get](../outdated/chats/imbot-dialog-get.md) | [imbot.v2.Chat.get](./imbot.v2/chats/chat-get.md) | Returns a Chat object ||
|| [imbot.chat.updateTitle](../outdated/chats/imbot-chat-update-title.md) | [imbot.v2.Chat.update](./imbot.v2/chats/chat-update.md) | In v2, changing the title is done through a universal chat properties update ||
|| [imbot.chat.updateColor](../outdated/chats/imbot-chat-update-color.md) | [imbot.v2.Chat.update](./imbot.v2/chats/chat-update.md) | In v2, changing the color is done through a universal chat properties update ||
|| [imbot.chat.updateAvatar](../outdated/chats/imbot-chat-update-avatar.md) | [imbot.v2.Chat.update](./imbot.v2/chats/chat-update.md) | In v2, changing the avatar is done through a universal chat properties update ||
|| — | [imbot.v2.Chat.update](./imbot.v2/chats/chat-update.md) | New universal method: update chat properties ||
|| [imbot.chat.user.add](../outdated/chats/imbot-chat-user-add.md) | [imbot.v2.Chat.User.add](./imbot.v2/chats/chat-user-add.md) | — ||
|| [imbot.chat.user.delete](../outdated/chats/imbot-chat-user-delete.md) | [imbot.v2.Chat.User.delete](./imbot.v2/chats/chat-user-delete.md) | — ||
|| [imbot.chat.user.list](../outdated/chats/imbot-chat-user-list.md) | [imbot.v2.Chat.User.list](./imbot.v2/chats/chat-user-list.md) | — ||
|| [imbot.chat.leave](../outdated/chats/imbot-chat-leave.md) | [imbot.v2.Chat.leave](./imbot.v2/chats/chat-leave.md) | — ||
|| [imbot.chat.setManager](../outdated/chats/imbot-chat-set-manager.md) | [imbot.v2.Chat.Manager.add](./imbot.v2/chats/chat-manager-add.md), [imbot.v2.Chat.Manager.delete](./imbot.v2/chats/chat-manager-delete.md) | In v2, assigning and removing admin rights are separated into two methods ||
|| [imbot.chat.setOwner](../outdated/chats/imbot-chat-set-owner.md) | [imbot.v2.Chat.setOwner](./imbot.v2/chats/chat-set-owner.md) | — ||
|| [imbot.chat.sendTyping](../outdated/chats/imbot-chat-send-typing.md) | [imbot.v2.Chat.InputAction.notify](./imbot.v2/ui/chat-input-action-notify.md) | — ||
|| — | [imbot.v2.Chat.TextField.enabled](./imbot.v2/ui/chat-text-field-enabled.md) | New method: control the input field ||
|| [imbot.command.register](../outdated/commands/imbot-command-register.md) | [imbot.v2.Command.register](./imbot.v2/commands/command-register.md) | Parameters in `fields.*` ||
|| [imbot.command.update](../outdated/commands/imbot-command-update.md) | [imbot.v2.Command.update](./imbot.v2/commands/command-update.md) | Parameters in `fields.*` ||
|| — | [imbot.v2.Command.list](./imbot.v2/commands/command-list.md) | New method: list of bot commands ||
|| [imbot.command.unregister](../outdated/commands/imbot-command-unregister.md) | [imbot.v2.Command.unregister](./imbot.v2/commands/command-unregister.md) | — ||
|| [imbot.command.answer](../outdated/commands/imbot-command-answer.md) | [imbot.v2.Command.answer](./imbot.v2/commands/command-answer.md) | — ||
|| — | [imbot.v2.Event.get](./imbot.v2/events/event-get.md) | New method: polling events (fetch mode) ||
|| — | [imbot.v2.File.upload](./imbot.v2/files/file-upload.md) | New method: upload a file to chat ||
|| — | [imbot.v2.File.download](./imbot.v2/files/file-download.md) | New method: get a download link ||
|| — | [imbot.v2.Revision.get](./imbot.v2/revision-get.md) | New method: get API revision numbers ||
|#

## Events

#|
|| **v1** | **v2** | **Changes** ||
|| [ONIMBOTMESSAGEADD](../outdated/messages/events/on-imbot-message-add.md) | [ONIMBOTV2MESSAGEADD](./imbot.v2/events/events.md#onimbotv2messageadd) | Data in V2 format (camelCase, Bot/Chat/Message/User objects) ||
|| [ONIMBOTMESSAGEUPDATE](../outdated/messages/events/on-imbot-message-update.md) | [ONIMBOTV2MESSAGEUPDATE](./imbot.v2/events/events.md#onimbotv2messageupdate) | Data in V2 format: camelCase and nested objects ||
|| [ONIMBOTMESSAGEDELETE](../outdated/messages/events/on-imbot-message-delete.md) | [ONIMBOTV2MESSAGEDELETE](./imbot.v2/events/events.md#onimbotv2messagedelete) | Data in V2 format: camelCase and nested objects ||
|| [ONIMBOTJOINCHAT](../outdated/chats/events/on-imbot-join-chat.md) | [ONIMBOTV2JOINCHAT](./imbot.v2/events/events.md#onimbotv2joinchat) | Data in V2 format: camelCase and nested objects ||
|| [ONIMBOTDELETE](../outdated/events/on-imbot-delete.md) | [ONIMBOTV2DELETE](./imbot.v2/events/events.md#onimbotv2delete) | Data in V2 format: camelCase and `bot` object in the new format ||
|| [ONIMCOMMANDADD](../outdated/commands/events/on-im-command-add.md) | [ONIMBOTV2COMMANDADD](./imbot.v2/events/events.md#onimbotv2commandadd) | Data in V2 format: camelCase and nested objects; `context` field in lowercase: `textarea`, `keyboard`, `menu` ||
|| ONIMBOTCONTEXTGET | [ONIMBOTV2CONTEXTGET](./imbot.v2/events/events.md#onimbotv2contextget) | Data in V2 format: camelCase and nested objects ||
|| — | [ONIMBOTV2REACTIONCHANGE](./imbot.v2/events/events.md#onimbotv2reactionchange) | New event: change reaction to a bot message ||
|#

## Key Differences in v2

### Data Format

- camelCase instead of UPPER_CASE for keys — for example, `chatId` instead of `CHAT_ID`
- Nested objects instead of flat fields — Bot, Chat, Message, User
- Boolean fields — actual `true`/`false` instead of strings `"Y"`/`"N"`

### Event Delivery

- v1: only webhook (EVENT_HANDLER URL)
- v2: fetch mode (polling via [imbot.v2.Event.get](./imbot.v2/events/event-get.md)) + webhook mode as an option when registering the bot

### Method Parameters

- v1: flat top-level parameters (`MESSAGE_ADD`, `BOT_ID`, `TYPE`, etc.)
- v2: grouping in `fields.*` / `properties.*`, camelCase names

### Authorization

- v1: OAuth or webhook authorization with `CLIENT_ID` field
- v2: OAuth or webhook authorization with `botToken`

## Changes within v2

The API `imbot.v2` continues to evolve. New features, fixes, and changes with loss of backward compatibility are published in the [API imbot.v2 Change Log](./change-log.md).

If the format of a method's call or response changes, the previous version will continue to be supported for **6 months** from the date of the change publication.

## Continue Exploring

- [{#T}](./imbot.v2/bots/bot-register.md)
- [{#T}](./imbot.v2/events/event-get.md)
- [API imbot.v2 Change Log](./change-log.md)
- [{#T}](../index.md)