# Migration from imbot to imbot.v2

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

This page helps you migrate an integration from the deprecated `imbot` API to `imbot.v2`: choose an event delivery mode, map methods and request fields, and update response handling.

{% note info "" %}

Methods v1 and v2 operate in parallel. Bots registered through v1 are visible in v2 and vice versa. However, the event formats differ — the bot receives events only in the format of the API version through which it was registered.

{% endnote %}

## Migration Steps {#migration-steps}

1. Find the v1 methods and events your integration uses in the tables below. Check each call's parameters against the corresponding v2 method page: field names, required fields, and allowed values may differ
2. Check which API version was used to register the bot. Replacing v1 calls with v2 calls does not change an existing bot's event format. If you need `ONIMBOTV2*` events, account for the registration version and configure handlers for the [new event format](./imbot.v2/events/events.md)
3. Choose a v2 event delivery mode using the [table below](#event-mode-choice)
4. Map request parameters using the methods table and check [authorization](#migration-auth)
5. Compare responses and event handlers with the v2 formats. Test the new call and event delivery before switching the live scenario. For a request and response comparison, see the [message sending example](#message-example)

## How to Choose an Event Mode {#event-mode-choice}

#|
|| **v2 Mode** | **When to Use It** | **What to Configure** ||
|| `fetch` | The application has no public event URL, or you need queue control through `offset` | Set `fields.eventMode: fetch` when registering the bot and poll [imbot.v2.Event.get](./imbot.v2/events/event-get.md). This is the default mode ||
|| `webhook` | The application has a public HTTP event handler | Set `fields.eventMode: webhook` and the required `fields.webhookUrl`. The handler must respond with HTTP 200; redelivery after a failure is not guaranteed ||
|#

Implementation details for both modes are in [Event Delivery Modes](./index.md#event-modes).

## Authorization During Migration {#migration-auth}

Methods in both versions use the [`imbot`](../../scopes/permissions.md) scope. With OAuth, v2 calls do not require `botToken`. With webhook authorization, pass the bot's `botToken` provided at registration: the v1 `CLIENT_ID` is a different token. Keep `botToken` out of public code and logs.

## Methods

#|
|| **v1** | **v2** | **Changes** ||
|| [imbot.register](../outdated/bots/imbot-register.md) | [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md) | `CODE` → `fields.code`, `PROPERTIES.NAME` → `fields.properties.name`, `PROPERTIES.WORK_POSITION` → `fields.properties.workPosition`. Instead of `EVENT_HANDLER`, set `fields.eventMode: webhook` and `fields.webhookUrl`; for polling, set `fields.eventMode: fetch` ||
|| [imbot.update](../outdated/bots/imbot-update.md) | [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md) | `BOT_ID` → `botId`, `FIELDS.PROPERTIES.NAME` → `fields.properties.name`, `FIELDS.PROPERTIES.WORK_POSITION` → `fields.properties.workPosition`, `FIELDS.EVENT_HANDLER` → `fields.webhookUrl` when `fields.eventMode: webhook`. Changing `webhookUrl` or `eventMode` automatically rebuilds `ONIMBOTV2*` subscriptions; manual `event.unbind` is not needed ||
|| [imbot.unregister](../outdated/bots/imbot-unregister.md) | [imbot.v2.Bot.unregister](./imbot.v2/bots/bot-unregister.md) | Subscriptions `ONIMBOTV2*` are cleaned automatically — workaround with manual `event.unbind` after unregister is not needed ||
|| [imbot.bot.list](../outdated/bots/imbot-bot-list.md) | [imbot.v2.Bot.list](./imbot.v2/bots/bot-list.md) | Returns an array of Bot objects instead of a flat list ||
|| — | [imbot.v2.Bot.get](./imbot.v2/bots/bot-get.md) | New method: retrieve a single bot by ID ||
|| [imbot.message.add](../outdated/messages/imbot-message-add.md) | [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md) | `BOT_ID` → `botId`, `DIALOG_ID` → `dialogId`, `MESSAGE` → `fields.message`, `ATTACH` → `fields.attach`, `KEYBOARD` → `fields.keyboard`, `SYSTEM` → `fields.system`, `URL_PREVIEW` → `fields.urlPreview`. In v2, `botId` is required ||
|| [imbot.message.update](../outdated/messages/imbot-message-update.md) | [imbot.v2.Chat.Message.update](./imbot.v2/messages/chat-message-update.md) | `BOT_ID` → `botId`, `MESSAGE_ID` → `messageId`, `MESSAGE` → `fields.message`, `ATTACH` → `fields.attach`, `KEYBOARD` → `fields.keyboard`, `URL_PREVIEW` → `fields.urlPreview` ||
|| [imbot.message.delete](../outdated/messages/imbot-message-delete.md) | [imbot.v2.Chat.Message.delete](./imbot.v2/messages/chat-message-delete.md) | No changes ||
|| — | [imbot.v2.Chat.Message.read](./imbot.v2/messages/chat-message-read.md) | New method: mark messages as read ||
|| — | [imbot.v2.Chat.Message.Reaction.add](./imbot.v2/messages/chat-message-reaction-add.md) | New method: add a reaction ||
|| — | [imbot.v2.Chat.Message.Reaction.delete](./imbot.v2/messages/chat-message-reaction-delete.md) | New method: remove a reaction ||
|| [imbot.message.like](../outdated/messages/imbot-message-like.md) | [imbot.v2.Chat.Message.Reaction.add](./imbot.v2/messages/chat-message-reaction-add.md), [imbot.v2.Chat.Message.Reaction.delete](./imbot.v2/messages/chat-message-reaction-delete.md) | In v2, setting and removing a reaction are separated into two methods ||
|| [imbot.chat.add](../outdated/chats/imbot-chat-add.md) | [imbot.v2.Chat.add](./imbot.v2/chats/chat-add.md) | `BOT_ID` → `botId`, `TYPE` → `fields.type`, `TITLE` → `fields.title`, `DESCRIPTION` → `fields.description`, `COLOR` → `fields.color`, `AVATAR` → `fields.avatar`, `USERS` → `fields.userIds`. In v2, `botId` is required, and `TYPE` and `COLOR` values have changed ||
|| [imbot.chat.get](../outdated/chats/imbot-chat-get.md) | [imbot.v2.Chat.get](./imbot.v2/chats/chat-get.md) | Returns a Chat object ||
|| [imbot.dialog.get](../outdated/chats/imbot-dialog-get.md) | [imbot.v2.Chat.get](./imbot.v2/chats/chat-get.md) | Returns a Chat object ||
|| [imbot.chat.updateTitle](../outdated/chats/imbot-chat-update-title.md) | [imbot.v2.Chat.update](./imbot.v2/chats/chat-update.md) | In v2, changing the title is done through a universal chat property update ||
|| [imbot.chat.updateColor](../outdated/chats/imbot-chat-update-color.md) | [imbot.v2.Chat.update](./imbot.v2/chats/chat-update.md) | In v2, changing the color is done through a universal chat property update ||
|| [imbot.chat.updateAvatar](../outdated/chats/imbot-chat-update-avatar.md) | [imbot.v2.Chat.update](./imbot.v2/chats/chat-update.md) | In v2, changing the avatar is done through a universal chat property update ||
|| — | [imbot.v2.Chat.update](./imbot.v2/chats/chat-update.md) | New universal method: update chat properties ||
|| [imbot.chat.user.add](../outdated/chats/imbot-chat-user-add.md) | [imbot.v2.Chat.User.add](./imbot.v2/chats/chat-user-add.md) | — ||
|| [imbot.chat.user.delete](../outdated/chats/imbot-chat-user-delete.md) | [imbot.v2.Chat.User.delete](./imbot.v2/chats/chat-user-delete.md) | — ||
|| [imbot.chat.user.list](../outdated/chats/imbot-chat-user-list.md) | [imbot.v2.Chat.User.list](./imbot.v2/chats/chat-user-list.md) | — ||
|| [imbot.chat.leave](../outdated/chats/imbot-chat-leave.md) | [imbot.v2.Chat.leave](./imbot.v2/chats/chat-leave.md) | — ||
|| [imbot.chat.setManager](../outdated/chats/imbot-chat-set-manager.md) | [imbot.v2.Chat.Manager.add](./imbot.v2/chats/chat-manager-add.md), [imbot.v2.Chat.Manager.delete](./imbot.v2/chats/chat-manager-delete.md) | In v2, assigning and removing admin rights are separated into two methods ||
|| [imbot.chat.setOwner](../outdated/chats/imbot-chat-set-owner.md) | [imbot.v2.Chat.setOwner](./imbot.v2/chats/chat-set-owner.md) | — ||
|| [imbot.chat.sendTyping](../outdated/chats/imbot-chat-send-typing.md) | [imbot.v2.Chat.InputAction.notify](./imbot.v2/ui/chat-input-action-notify.md) | — ||
|| — | [imbot.v2.Chat.TextField.enabled](./imbot.v2/ui/chat-text-field-enabled.md) | New method: control the input field ||
|| [imbot.command.register](../outdated/commands/imbot-command-register.md) | [imbot.v2.Command.register](./imbot.v2/commands/command-register.md) | `BOT_ID` → `botId`, `COMMAND` → `fields.command`, `LANG[].TITLE` → `fields.title.{language_code}`, `LANG[].PARAMS` → `fields.params.{language_code}`, `COMMON` → `fields.common`, `HIDDEN` → `fields.hidden`, `EXTRANET_SUPPORT` → `fields.extranetSupport` ||
|| [imbot.command.update](../outdated/commands/imbot-command-update.md) | [imbot.v2.Command.update](./imbot.v2/commands/command-update.md) | `COMMAND_ID` → `commandId`, `FIELDS.COMMAND` → `fields.command`, `FIELDS.LANG[].TITLE` → `fields.title.{language_code}`, `FIELDS.LANG[].PARAMS` → `fields.params.{language_code}`, `FIELDS.HIDDEN` → `fields.hidden`, `FIELDS.EXTRANET_SUPPORT` → `fields.extranetSupport`. In v2, `botId` is also required ||
|| — | [imbot.v2.Command.list](./imbot.v2/commands/command-list.md) | New method: list of bot commands ||
|| [imbot.command.unregister](../outdated/commands/imbot-command-unregister.md) | [imbot.v2.Command.unregister](./imbot.v2/commands/command-unregister.md) | — ||
|| [imbot.command.answer](../outdated/commands/imbot-command-answer.md) | [imbot.v2.Command.answer](./imbot.v2/commands/command-answer.md) | — ||
|| — | [imbot.v2.Event.get](./imbot.v2/events/event-get.md) | New method: polling events (fetch mode) ||
|| — | [imbot.v2.File.upload](./imbot.v2/files/file-upload.md) | New method: upload a file to chat ||
|| — | [imbot.v2.File.download](./imbot.v2/files/file-download.md) | New method: get a download link ||
|| — | [imbot.v2.Revision.get](./imbot.v2/revision-get.md) | New method: get API revision numbers ||
|#

The table lists fields with equivalents in the specified v2 methods. For `boolean` fields, convert `Y`/`N` to `true`/`false`. When creating a chat, `TYPE=CHAT` corresponds to `fields.type=chat`, and `TYPE=OPEN` to `fields.type=open`. Some v1 parameters have no direct field in the corresponding v2 method, such as `EVENT_COMMAND_ADD` in `Command.register` and `MENU` in `Chat.Message.send`.

## Example: Sending a Message in v1 and v2 {#message-example}

The OAuth requests and responses below come from the [imbot.message.add](../outdated/messages/imbot-message-add.md) and [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md) method pages. These are independent examples with different IDs and message text.

**v1.** Pass the message text in the top-level `MESSAGE` field:

```json
{
    "BOT_ID": 39,
    "DIALOG_ID": "chat123",
    "MESSAGE": "Message text",
    "auth": "**put_access_token_here**"
}
```

The v1 response returns the numeric message ID in `result`:

```json
{
    "result": 19880117,
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+03:00",
        "date_finish": "2024-10-11T10:00:00+03:00",
        "operating_reset_at": 1762349466,
        "operating": 0
    }
}
```

**v2.** Pass the message text in the nested `fields.message` field:

```json
{
    "botId": 456,
    "dialogId": "chat5",
    "fields": {
        "message": "Hello from bot!"
    },
    "auth": "**put_access_token_here**"
}
```

The v2 response returns the message ID in `result.id`:

```json
{
    "result": {
        "id": 789,
        "uuidMap": {}
    },
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+03:00",
        "date_finish": "2024-10-11T10:00:00+03:00"
    }
}
```

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
- Some flags use `true`/`false` instead of strings `"Y"`/`"N"`, for example a command's `fields.common`. Check the type on the method page: `fields.urlPreview` in [Chat.Message.update](./imbot.v2/messages/chat-message-update.md) accepts `Y`/`N`

### Event Delivery

In v1, events are delivered through a webhook. In v2, both `fetch` and `webhook` are available; the [mode selection criteria](#event-mode-choice) are described above.

### Method Parameters

v1 parameters are passed at the top level (`MESSAGE`, `BOT_ID`, `TYPE`, and others). v2 uses camelCase names and nested `fields.*` fields, including `fields.properties.*` for bot properties. The methods table gives specific mappings.

### Authorization

v1 supports OAuth and webhook authorization with `CLIENT_ID`; v2 supports OAuth or webhook authorization with `botToken`. Token requirements are described in [Authorization During Migration](#migration-auth).

## Changes within v2

The API `imbot.v2` continues to evolve. New features, fixes, and changes with loss of backward compatibility are published in the [API imbot.v2 Change Log](./change-log.md).

If the method call or response format changes, the previous version will continue to be supported for **6 months** from the date of the change publication.

## Continue Learning

- [{#T}](./imbot.v2/bots/bot-register.md)
- [{#T}](./imbot.v2/events/event-get.md)
- [API imbot.v2 Change Log](./change-log.md)
- [{#T}](../index.md)
