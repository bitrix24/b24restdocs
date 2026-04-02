# Chatbots 2.0: Quick Start

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute methods: owner of the registered bot

A brief scenario for launching a chatbot on `imbot.v2`: creating a webhook, registering a bot, receiving events, sending messages, and working with files.

## Creating an Incoming Webhook {#webhook-create}

To get started quickly, create an incoming webhook in the Bitrix24 interface:

1. Go to `Developer resources -> Other -> Incoming webhook`.
2. In the permissions, select the scope `imbot`.
3. Save and copy the webhook URL.

URL format:

```text
https://{account}/rest/{user_id}/{webhook_token}/
```

## Typical Use-Case {#scenario}

### 1. Register a Bot {#register-bot}

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

### 5. Upload a File in Chat {#files}

Use [imbot.v2.File.upload](./imbot.v2/files/file-upload.md) to send a file in chat on behalf of the bot.

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

### 6. Get Download Link for a File

Use [imbot.v2.File.download](./imbot.v2/files/file-download.md) to obtain the URL for downloading a file.

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

## Continue Learning

- [Chatbots 2.0: Overview of Methods](./index.md)
- [Register a Bot imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md)
- [Get Bot Events imbot.v2.Event.get](./imbot.v2/events/event-get.md)
- [Event Formats for imbot.v2](./imbot.v2/events/events.md)
- [Send Message imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md)
- [Objects and Fields of Chatbots 2.0](./entities.md)
- [Migration from imbot to imbot.v2](./migration.md)