# Chatbots 2.0: Quick Start

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute methods: owner of the registered bot

A brief scenario for launching a chatbot on `imbot.v2`: creating a webhook, registering a bot, receiving events, sending messages, and working with files.

{% note info "" %}

Before you begin, check the [API Change Log for imbot.v2](./change-log.md). It contains new features, fixes, and breaking changes, with entries listed from newest to oldest.

{% endnote %}

## Creating an Incoming Webhook {#webhook-create}

To get started quickly, create an incoming webhook in the Bitrix24 interface:

1. Go to `Developer resources -> Other -> Incoming Webhook`.
2. In the permissions, select the scope `imbot`.
3. Save and copy the webhook URL.

URL format:

```text
https://{account}/rest/{user_id}/{webhook_token}/
```

## Typical Use-case {#scenario}

### 1. Register the Bot {#register-bot}

Use the method [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md) to create a bot and set its main properties.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.Bot.register' \
  -H 'Content-Type: application/json' \
  -d '{
    "botToken": "my_secret_token_123",
    "fields": {
      "code": "support_bot",
      "properties": {"name": "Support Bot", "workPosition": "AI Assistant"},
      "eventMode": "fetch"
    }
  }'
```

### 2. Get Events (fetch mode) {#get-events}

Use [imbot.v2.Event.get](./imbot.v2/events/event-get.md) to retrieve the event queue for the registered bot.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.Event.get' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 456,
    "botToken": "my_secret_token_123",
    "limit": 50
  }'
```

### 3. Respond in Chat {#send-message}

Use [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md) to send a response in the dialogue.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.Chat.Message.send' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 456,
    "botToken": "my_secret_token_123",
    "dialogId": "chat5",
    "fields": {"message": "Hello! How can I help you?"}
  }'
```

### 4. Read Message by `replyId` (only `supervisor`/`personal`)

If a user replies to the bot's message, retrieve the original message using [imbot.v2.Chat.Message.get](./imbot.v2/messages/chat-message-get.md).

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.Chat.Message.get' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 456,
    "botToken": "my_secret_token_123",
    "messageId": 789
  }'
```

### 5. Upload File to Chat {#files}

Use [imbot.v2.File.upload](./imbot.v2/files/file-upload.md) to send a file in the chat on behalf of the bot.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.File.upload' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 456,
    "botToken": "my_secret_token_123",
    "dialogId": "chat5",
    "fields": {"name": "report.txt", "content": "SGVsbG8gV29ybGQh", "message": "Here is the report"}
  }'
```

### 6. Get Download Link for File

Use [imbot.v2.File.download](./imbot.v2/files/file-download.md) to obtain the URL for downloading the file.

```bash
curl -X POST 'https://example.bitrix24.com/rest/1/webhook_token/imbot.v2.File.download' \
  -H 'Content-Type: application/json' \
  -d '{
    "botId": 456,
    "botToken": "my_secret_token_123",
    "dialogId": "chat5",
    "fileId": 138
  }'
```

## Additional Messaging Features

When sending messages through [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md), the following features are available:

- [Text Formatting (BB Codes)](./imbot.v2/messages/message-formatting.md): bold, italic, links, quotes, code, and other BB codes
- [Attachments (Attach)](./imbot.v2/messages/attachments/index.md): structured blocks with images, tables, grids, and other elements
- [Keyboards (Keyboard)](./imbot.v2/messages/message-keyboards.md): interactive buttons below the message

## API Revisions and Compatibility

Bitrix24 cloud and on-premise versions may have different API revisions. To find out which revision is installed on a specific account, use [imbot.v2.Revision.get](./imbot.v2/revision-get.md).

New features, fixes, and changes with loss of backward compatibility are compiled on the [API Change Log for imbot.v2](./change-log.md). If the integration is already working in production, this page should be checked first.

## Learning Path

1. [API Change Log for imbot.v2](./change-log.md)
2. [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md)
3. [imbot.v2.Event.get](./imbot.v2/events/event-get.md)
4. [imbot.v2 Events](./imbot.v2/events/events.md)
5. [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md)
6. [imbot.v2.Chat.Message.get](./imbot.v2/messages/chat-message-get.md) and [imbot.v2.Chat.Message.getContext](./imbot.v2/messages/chat-message-get-context.md)
7. [imbot.v2.Command.register](./imbot.v2/commands/command-register.md)
8. [imbot.v2.File.upload](./imbot.v2/files/file-upload.md) and [imbot.v2.File.download](./imbot.v2/files/file-download.md)
9. [imbot.v2.Chat.add](./imbot.v2/chats/chat-add.md)

## Continue Learning

- [API Change Log for imbot.v2](./change-log.md)
- [{#T}](./index.md)
- [{#T}](./imbot.v2/bots/bot-register.md)
- [{#T}](./imbot.v2/events/event-get.md)
- [{#T}](./imbot.v2/events/events.md)
- [{#T}](./imbot.v2/messages/chat-message-send.md)
- [{#T}](./entities.md)
- [{#T}](./migration.md)