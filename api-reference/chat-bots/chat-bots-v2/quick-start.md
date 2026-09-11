# Chatbots 2.0: Quick Start

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute methods: an authorized user registers the bot; the owner of the registered bot executes the remaining methods in this scenario

A short scenario for launching a chatbot on `imbot.v2`: creating a webhook, registering the bot, creating a chat, receiving events, sending messages, and working with files. After each call, the response is shown along with the field that goes into the next step.

{% note info "" %}

Before you begin, check the [API imbot.v2 Change Log](./change-log.md). It contains new features, fixes, and breaking changes, with entries listed from newest to oldest.

{% endnote %}

## Creating an Inbound Webhook {#webhook-create}

To get started quickly, create an inbound webhook in the Bitrix24 interface:

1. Go to `Developer resources -> Other -> Inbound Webhook`.
2. In the permissions, select the scope `imbot`.
3. Save and copy the webhook URL.

URL format:

```text
https://{account}/rest/{user_id}/{webhook_token}/
```

The webhook URL contains `{webhook_token}` — it is a secret: it grants access to Bitrix24 within the selected permissions. Keep it out of public code, logs, and examples, like any access key.

## What You Will Need

Values used throughout the scenario:

- `botToken` — the bot token. Choose it yourself when registering the bot and pass it in all subsequent `imbot.v2` calls via the webhook. It is a secret, so keep it like an access key. The maximum length is 40 characters; see the details on the [{#T}](./imbot.v2/bots/bot-register.md) page
- `botId` — the bot ID. It comes in the registration response; you do not need to make it up
- `dialogId` — the dialog ID. For a group chat, it comes ready-made in the chat creation response and looks like `chat5235`. For a private conversation, it is the user ID

The numeric values in the examples below were retrieved on a test Bitrix24 — yours will be different.

## Typical Use-case {#scenario}

Before you start, choose a route — it determines the bot type in step 1:

- **regular bot.** Register it with `"type": "bot"` and skip step 5: reading messages is unavailable for this type
- **supervisor bot.** Register it with `"type": "supervisor"` and go through all seven steps

The bot types and their behavior are described in the [{#T}](./index.md) article.

The chain of methods from registration to a file in the chat:

1. [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md) — create the bot, retrieve `botId`
2. [imbot.v2.Chat.add](./imbot.v2/chats/chat-add.md) — create a chat, retrieve `dialogId`
3. [imbot.v2.Event.get](./imbot.v2/events/event-get.md) — fetch the event queue
4. [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md) — reply in the dialog, retrieve `messageId`
5. [imbot.v2.Chat.Message.get](./imbot.v2/messages/chat-message-get.md) — read a message by ID, optional step
6. [imbot.v2.File.upload](./imbot.v2/files/file-upload.md) — send a file, retrieve `fileId`
7. [imbot.v2.File.download](./imbot.v2/files/file-download.md) — retrieve the download link

## 1. Register a Bot {#register-bot}

Use the [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md) method to create a bot and set its main properties. Pass the token inside `fields` — this is how the method contract expects it.

In all the examples below, replace `https://example.bitrix24.com/rest/1/webhook_token/` with the webhook URL copied in the previous step.

The `eventMode` parameter sets the event delivery mode. The `fetch` value means the bot fetches events itself — step 3 works with it. The other mode, `webhook`, requires a public handler URL and is not used in this scenario.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.Bot.register' \
  -H 'Content-Type: application/json' \
  -d '{
    "fields": {
      "code": "support_bot",
      "botToken": "my_secret_token_123",
      "type": "bot",
      "eventMode": "fetch",
      "properties": {"name": "Support Bot", "workPosition": "AI Assistant"}
    }
  }'
```

Successful response, fields abbreviated:

```json
{
  "result": {
    "bot": {
      "id": 1529,
      "code": "support_bot",
      "type": "bot",
      "eventMode": "fetch"
    },
    "users": [
      {"id": 1529, "name": "Support Bot", "bot": true}
    ]
  }
}
```

Save `result.bot.id` — this is the `botId` for all subsequent calls. For the full response and field tables, see the method page.

The example registers a regular bot. For the supervisor route, specify `"type": "supervisor"` — the same type is returned in the response, and step 5 becomes available.

## 2. Create a Chat {#create-chat}

So that the bot has somewhere to write, create a chat with the [imbot.v2.Chat.add](./imbot.v2/chats/chat-add.md) method. If the bot replies in an existing dialog, you can skip this step.

List the participants in the `userIds` field — these are the IDs of Bitrix24 employees. You can retrieve your own ID with the [user.current](../../user/user-current.md) method and the list of employees with the [user.get](../../user/user-get.md) method.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.Chat.add' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 1529,
    "botToken": "my_secret_token_123",
    "fields": {"title": "Support chat", "userIds": [1295]}
  }'
```

Successful response, fields abbreviated:

```json
{
  "result": {
    "chat": {
      "id": 5235,
      "dialogId": "chat5235",
      "name": "Support chat",
      "type": "chat"
    }
  }
}
```

Save `result.chat.dialogId` — you do not need to assemble it manually, it comes ready-made.

{% note warning "" %}

Pass the participants only in the `userIds` field. The method silently accepts the `users` field: it returns `200` and creates the chat, but the employees are not added to it — only the bot remains in the chat. There is no error in this case

{% endnote %}

## 3. Receive Events in Fetch Mode {#get-events}

Use [imbot.v2.Event.get](./imbot.v2/events/event-get.md) to fetch the event queue for the registered bot. In fetch mode, events accumulate on the Bitrix24 side, and the application fetches them itself — no handler or public URL is needed for this.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.Event.get' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 1529,
    "botToken": "my_secret_token_123",
    "limit": 50
  }'
```

Until someone writes to the bot, the queue is empty:

```json
{
  "result": {
    "events": [],
    "nextOffset": 0,
    "hasMore": false
  }
}
```

When a user writes to the bot, the `ONIMBOTV2MESSAGEADD` event appears in the queue. The response is abbreviated to the fields needed for a reply:

```json
{
  "result": {
    "events": [
      {
        "eventId": 401,
        "type": "ONIMBOTV2MESSAGEADD",
        "date": "2026-09-10T22:08:37+02:00",
        "data": {
          "message": {
            "id": 41047,
            "chatId": 5245,
            "authorId": 1295,
            "text": "Hello! I need help with my order"
          },
          "chat": {
            "id": 5245,
            "dialogId": "1295",
            "type": "private"
          },
          "user": {
            "id": 1295,
            "bot": false
          }
        }
      }
    ],
    "nextOffset": 402,
    "hasMore": false
  }
}
```

What to take from the event:

- `data.chat.dialogId` — the address for the reply; it goes into the next call
- `data.message.text` — the text the bot replies to
- `data.message.id` — the ID of the user's message, in case you need to read it separately
- `data.user.bot` — a flag showing that the message author is a bot. You can use it to filter out events from bots if your bot should reply only to people

To fetch the queue further, pass `result.nextOffset` in the next request as the `offset` parameter:

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.Event.get' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 1529,
    "botToken": "my_secret_token_123",
    "offset": 402,
    "limit": 50
  }'
```

The `offset` value confirms that all events with smaller IDs have been processed: without it, the same events arrive again. The parameter is not passed in the first call. Repeat the call until `hasMore` becomes `false`.

{% note info "" %}

In a private conversation, the bot receives every message. In a group chat, events do not arrive for every message — the set of events and the conditions for sending them are described in the [{#T}](./imbot.v2/events/events.md) article

{% endnote %}

## 4. Reply in the Chat {#send-message}

Use [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md) to send a response in the dialogue.

Put the dialog address in `dialogId`. If the bot replies to a message, this is `data.chat.dialogId` from the event in step 3. If the bot writes first, it is `result.chat.dialogId` from step 2.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.Chat.Message.send' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 1529,
    "botToken": "my_secret_token_123",
    "dialogId": "chat5235",
    "fields": {"message": "Hello! How can I help you?"}
  }'
```

Successful response, fields abbreviated:

```json
{
  "result": {
    "id": 41017
  }
}
```

The `result.id` value is the `messageId` of the sent message. You will need it to edit or delete the message, or to read it in the next step.

## 5. Read a Message by ID {#read-message}

This step is available only on the supervisor route: the [imbot.v2.Chat.Message.get](./imbot.v2/messages/chat-message-get.md) method works for the `supervisor` and `personal` types, while a bot with `"type": "bot"` gets the `BOT_TYPE_NOT_ALLOWED` error. The example below continues the scenario in which `"type": "supervisor"` was specified in step 1.

The method reads a message by `messageId`. This can be the ID from step 4 or the ID of the user's message retrieved from the event.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.Chat.Message.get' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 1529,
    "botToken": "my_secret_token_123",
    "messageId": 41017
  }'
```

Successful response, fields abbreviated:

```json
{
  "result": {
    "message": {
      "id": 41017,
      "chatId": 5235,
      "authorId": 1529,
      "text": "Hello! How can I help you?"
    },
    "user": {
      "id": 1529,
      "name": "Support Bot",
      "bot": true
    }
  }
}
```

`result.message` contains the message itself, and `result.user` contains its author.

## 6. Upload a File to the Chat {#files}

Use [imbot.v2.File.upload](./imbot.v2/files/file-upload.md) to send a file to the chat on behalf of the bot. The file content is passed as a Base64 string in the `content` field.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.File.upload' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 1529,
    "botToken": "my_secret_token_123",
    "dialogId": "chat5235",
    "fields": {"name": "report.txt", "content": "SGVsbG8gV29ybGQh", "message": "Here is the report"}
  }'
```

Successful response, fields abbreviated:

```json
{
  "result": {
    "file": {
      "id": 10021,
      "name": "report.txt",
      "extension": "txt",
      "size": 12,
      "authorId": 1529
    },
    "messageId": 41019,
    "chatId": 5235,
    "dialogId": "chat5235"
  }
}
```

Save `result.file.id` — this is the `fileId` for the next step. `result.messageId` contains the ID of the message used to send the file to the chat.

## 7. Retrieve a File Download Link {#download}

Use [imbot.v2.File.download](./imbot.v2/files/file-download.md) to obtain the URL for downloading the file.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.File.download' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 1529,
    "botToken": "my_secret_token_123",
    "fileId": 10021
  }'
```

Successful response:

```json
{
  "result": {
    "downloadUrl": "https://example.bitrix24.com/rest/1/webhook_token/download/?token=imbot%7C..."
  }
}
```

The link in `result.downloadUrl` is single-use: it contains a file access token, and reuse is not guaranteed. Do not publish it or store it in publicly accessible places — if you need the link again, request it anew.

## Checking the Result {#check}

The scenario has completed successfully if four conditions are met:

- the registration returned `result.bot.id`, and the remaining calls work with this `botId`
- the bot appears in the response of the [imbot.v2.Bot.list](./imbot.v2/bots/bot-list.md) method under the code from `fields.code`
- the message from step 4 is visible in the chat on behalf of the bot
- the file from step 6 can be opened using the link from `result.downloadUrl`

When the bot is no longer needed, delete it with the [imbot.v2.Bot.unregister](./imbot.v2/bots/bot-unregister.md) method — otherwise it remains registered on this Bitrix24.

## Errors and Diagnostics {#errors}

If a method returned an error, check the request data.

#|
|| **Error Code** | **Cause and What to Do** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | The request has no `botToken`. With webhook authorization, it is required for all `imbot.v2` methods. In registration, the token is passed inside `fields`; in the remaining calls — at the top level ||
|| `BOT_OWNERSHIP_ERROR` | The bot belongs to another application. When working via a webhook, the same error comes if `botToken` does not match the one set at registration — check the token ||
|| `BOT_NOT_FOUND` | The `botId` of a nonexistent bot is specified. Take the value from `result.bot.id` of the registration response ||
|| `BOT_CODE_ALREADY_TAKEN` | A bot with this `code` is already registered under a different `botToken`. Set a different code or delete the previous bot. With the same token there is no error: the method returns the existing bot instead of creating a new one ||
|| `BOT_TYPE_NOT_ALLOWED` | The method is available only to bots of the `supervisor` and `personal` types. Check `type` in the registration response ||
|| `FILE_NOT_FOUND` | There is no file with this `fileId`. Take the value from `result.file.id` of the upload response ||
|| `EMPTY_MESSAGE` | The `message` field is empty. Pass non-empty text ||
|#

Errors in steps 2–7 do not affect the bot registration: fix the request and repeat the same step. Go back to step 1 only if the bot is not registered or its code is already taken.

Separately, check for a silent failure — no error, but a wrong result: the chat is created, but no employees are added to it. This happens when `users` is passed instead of `userIds` in step 2. The list of participants is shown by the [imbot.v2.Chat.User.list](./imbot.v2/chats/chat-user-list.md) method.

## Additional Messaging Features

When sending messages via [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md), the following are available:

- [Text Formatting (BB Codes)](./imbot.v2/messages/message-formatting.md): bold, italic, links, quotes, code, and other BB codes
- [Attachments (Attach)](./imbot.v2/messages/attachments/index.md): structured blocks with images, tables, grids, and other elements
- [Keyboards (Keyboard)](./imbot.v2/messages/message-keyboards.md): interactive buttons below the message

## API Revisions and Compatibility

Bitrix24 cloud and on-premise versions may have different API revisions. To find out which revision is installed in a specific Bitrix24, use [imbot.v2.Revision.get](./imbot.v2/revision-get.md).

New features, fixes, and changes with loss of backward compatibility are compiled on the [API imbot.v2 Change Log](./change-log.md). If the integration is already working in production, this page should be checked first.

## Continue Learning

- [API imbot.v2 Change Log](./change-log.md)
- [{#T}](./index.md)
- [{#T}](./imbot.v2/events/events.md)
- [{#T}](./imbot.v2/messages/chat-message-get-context.md)
- [{#T}](./imbot.v2/commands/command-register.md)
- [{#T}](./entities.md)
- [{#T}](./migration.md)