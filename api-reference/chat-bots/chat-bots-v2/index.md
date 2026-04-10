# Chatbots 2.0: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Chatbots 2.0 combine two sets of APIs:
— The `imbot.v2` methods manage the bot's lifecycle, messages, commands, files, and chat management.
— The `im.v2` methods allow applications or users to subscribe to messenger events and receive them in polling mode—where the application periodically requests accumulated events from the server without needing a public URL for incoming requests.

> Quick navigation: [Quick Start](./quick-start.md) | [Change Log](./change-log.md) | [Authorization](#auth) | [Bot Types](#bot-types) | [Events](#event-modes) | [Limits](#limits) | [All Methods](#all-methods)

## What Tasks Does This Section Solve

- Register and configure a chatbot
- Receive events from the bot platform or messenger
- Send and modify messages
- Manage group chats and participants
- Upload and download files
- Register slash commands and respond to them

{% note info "" %}

If the integration is already live or uses new fields and methods, first check the [Change Log for API imbot.v2](./change-log.md). Entries are listed from newest to oldest.

{% endnote %}

## How to Get Started {#how-to-start}

A typical workflow with the new API:

1. Register the bot via [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md).
2. Set up event reception via [imbot.v2.Event.get](./imbot.v2/events/event-get.md) or [im.v2.Event.subscribe](./im.v2/events/event-subscribe.md).
3. Send a message using [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md).
4. Read the original message by `replyId` through [imbot.v2.Chat.Message.get](./imbot.v2/messages/chat-message-get.md)—only for `supervisor` and `personal` type bots.
5. Get the dialogue context via [imbot.v2.Chat.Message.getContext](./imbot.v2/messages/chat-message-get-context.md)—only for `supervisor` and `personal` type bots.
6. Upload files via [imbot.v2.File.upload](./imbot.v2/files/file-upload.md) and download them via [imbot.v2.File.download](./imbot.v2/files/file-download.md).

For a detailed step-by-step scenario with cURL examples: [Quick Start](./quick-start.md).

## Authorization {#auth}

Chatbots 2.0 support two authorization methods.

### Webhook

The URL contains the authorization token. This option is suitable for local integrations, AI agents, and testing.

```text
POST https://{account}/rest/{user_id}/{webhook_token}/{method_name}
Content-Type: application/json
```

In webhook authorization, the `botToken` parameter is required for `imbot.v2` methods. This is a random string that you set when registering the bot and then use in all calls on behalf of that bot.

### OAuth

The token is passed as the `auth` parameter. This option is suitable for applications from the Marketplace and internal applications.

```text
POST https://{account}/rest/{method_name}?auth={access_token}
Content-Type: application/json
```

In OAuth authorization, the `botToken` parameter for `imbot.v2` is not needed because the bot is tied to the application via `client_id`.

## Bot Types {#bot-types}

The `fields.type` parameter when registering the bot determines its behavior in chats.

#| 
|| **Type** | **Description** | **Event Visibility in Group Chats** ||
|| `bot` | Regular bot. Responds to mentions of `@bot` in group chats and direct messages | Receives events only when mentioned `@bot` or in one-on-one dialogue ||
|| `supervisor` | System observer. Sees all messages in chats where it is present | Receives all events without mention ||
|| `personal` | Personal assistant. Sees all messages in chats where it is present | Receives all events without mention—similar to `supervisor`. Searching for such a bot among chats is restricted for certain user groups ||
|| `openline` | Bot for Open Channels | Behavior is similar to `bot` ||
|#

The default type `bot` is suitable for most scenarios.

The `personal` type is recommended for AI assistants that require full context of group dialogue. Bots of this type will be hidden from search for users who do not have access to them in the future.

{% note warning "" %}

Bots of types `personal` and `supervisor` receive a stream of all messages in the chats they are part of. The bot filters out irrelevant messages on its own.

{% endnote %}

## Response Format

All methods return a JSON response in a unified wrapper:

```json
{
    "result": {},
    "time": {
        "start": 1700000000,
        "finish": 1700000000.5,
        "duration": 0.5,
        "date_start": "2025-01-15T10:00:00+01:00",
        "date_finish": "2025-01-15T10:00:00+01:00"
    }
}
```

- `result` — data returned by the method
- `time` — metadata about execution time

In case of an error, instead of `result`, it returns:

```json
{
    "error": "ERROR_CODE",
    "error_description": "Human-readable description"
}
```

The `error` field is stable and suitable for programmatic processing (switch/case). The `error_description` field is intended for logging—its text may change.

### Example Error Handling

```php
$result = CRest::call('imbot.v2.Chat.Message.send', [
    'botId'    => $botId,
    'botToken' => $botToken,
    'dialogId' => $dialogId,
    'fields'   => ['message' => 'Hello'],
]);

if (isset($result['error'])) {
    switch ($result['error']) {
        case 'BOT_NOT_FOUND':
            // Bot deleted — re-register
            break;
        case 'ACCESS_DENIED':
            // Bot not in chat — skip
            break;
        default:
            error_log('API error: ' . $result['error'] . ' — ' . ($result['error_description'] ?? ''));
    }
}
```

Error codes for each method are listed in the section "Possible Error Codes" on the method's page.

## Boolean Parameters

Boolean parameters accept `true` / `false`. If your client does not support sending JSON booleans, you can pass the strings `"Y"` / `"N"`—the API accepts both options.

## Format of dialogId {#dialog-id}

The `dialogId` parameter is a universal identifier for the dialogue. It is used in methods for working with chats, messages, and files.

#| 
|| **Chat Type** | **Format** | **Example** | **Description** ||
|| Personal (P2P) | `"{userId}"` | `"5"` | String with the user ID. A dialogue with the bot is created automatically ||
|| Group | `"chat{chatId}"` | `"chat142"` | String with the prefix `chat` and chat ID ||
|#

In personal chats, `dialogId` always contains the ID of the **counterparty**. If user 1 writes to bot 5—the bot in the event will receive `dialogId = "1"`, and the user will receive `dialogId = "5"`. Use the `dialogId` from the event for responses—it already points to the correct dialogue.

`dialogId` is used in methods for working with chats, messages, and files such as `Chat.Message.send`, `Chat.get`, `File.upload`, etc. Other methods use their own identifiers: `botId` (Bot.update, Event.get), `commandId` (Command.update), `fileId` (File.download). The methods `im.v2.Event.*` do not use `dialogId`.

## Event Delivery Modes {#event-modes}

When registering the bot, the `eventMode` is selected.

#| 
|| **Mode** | **Description** | **When to Use** ||
|| `fetch` | The bot retrieves events itself via [imbot.v2.Event.get](./imbot.v2/events/event-get.md) | For AI agents, server bots, and integrations without a permanent HTTP server ||
|| `webhook` | Bitrix24 sends events to the bot's URL via HTTP POST | For bots with a dedicated HTTP server ||
|#

### Polling Cycle (fetch mode) {#polling}

A typical polling cycle for a bot with `eventMode: "fetch"`:

```php
$offset = 0;        // first call without offset — get all accumulated events
$pollInterval = 10; // interval in seconds when there are no new events

while (true) {
    $result = CRest::call('imbot.v2.Event.get', [
        'botId'    => $botId,
        'botToken' => $botToken,
        'offset'   => $offset,
        'limit'    => 100,
    ]);

    if (!empty($result['error'])) {
        // exponential backoff on errors and HTTP 429
        error_log('Event.get error: ' . $result['error']);
        sleep(min($pollInterval * 2, 60));
        continue;
    }

    foreach ($result['result']['events'] ?? [] as $event) {
        processEvent($event['type'], $event['data']);
    }

    // Confirm processed events — the next call will return only new ones
    if (!empty($result['result']['nextOffset'])) {
        $offset = $result['result']['nextOffset'];
    }

    // If there are more events — poll immediately, otherwise wait
    sleep(!empty($result['result']['hasMore']) ? 2 : $pollInterval);
}
```

Recommended interval: **5–30 seconds** when there are no new events. If `hasMore = true` — the next request should have a minimum pause of 2 seconds.

### Webhook Handler (webhook mode) {#webhook}

Example handler for receiving events in `eventMode: "webhook"`. The handler URL is passed in the `fields.webhookUrl` parameter when registering the bot.

```php
// webhook_handler.php
$data = json_decode(file_get_contents('php://input'), true);

if (empty($data['event'])) {
    http_response_code(400);
    exit;
}

$eventType = $data['event'];
$eventData = $data['data'] ?? [];

// In webhook mode, all scalar values come as strings—cast types explicitly
switch ($eventType) {
    case 'ONIMBOTV2MESSAGEADD':
        $botId    = (int)($eventData['bot']['id'] ?? 0);
        $text     = $eventData['message']['text'] ?? '';
        $dialogId = $eventData['chat']['dialogId'] ?? '';
        CRest::call('imbot.v2.Chat.Message.send', [
            'botId'    => $botId,
            'dialogId' => $dialogId,
            'fields'   => ['message' => 'Received: ' . $text],
        ]);
        break;

    case 'ONIMBOTV2COMMANDADD':
        $botId     = (int)($eventData['bot']['id'] ?? 0);
        $commandId = (int)($eventData['command']['id'] ?? 0);
        $messageId = (int)($eventData['message']['id'] ?? 0);
        CRest::call('imbot.v2.Command.answer', [
            'botId'     => $botId,
            'commandId' => $commandId,
            'messageId' => $messageId,
            'dialogId'  => $eventData['chat']['dialogId'] ?? '',
            'fields'    => ['message' => 'Command executed'],
        ]);
        break;

    case 'ONIMBOTV2JOINCHAT':
        // Bot added to chat — send a greeting
        break;

    case 'ONIMBOTV2DELETE':
        // Bot removed — clean up resources
        break;
}

http_response_code(200);
echo json_encode(['status' => 'ok']);
```

The platform expects an HTTP 200 response from the handler. In case of delivery failure, retries are not guaranteed—use fetch mode for reliability.

## Additional Messaging Features

When sending messages via [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md), the following are available:

- [Text Formatting (BB Codes)](./imbot.v2/messages/message-formatting.md) — bold, italic, links, quotes, code, and other BB codes
- [Attachments (Attach)](./imbot.v2/messages/attachments/index.md) — structured blocks: images, tables, grids, and other elements
- [Keyboards (Keyboard)](./imbot.v2/messages/message-keyboards.md) — interactive buttons below the message

## Limits {#limits}

General limits of the Bitrix24 REST API also apply to the bot platform methods. More details: [REST API Limits](../../../limits.md).

#| 
|| **Limit** | **Value** ||
|| Rate limit | 2 requests per second per application ||
|| When exceeding rate limit | HTTP 429 — use exponential backoff ||
|| Number of bots per application | 100 ||
|| File size for `File.upload` | 100 MB ||
|| Message length | 20,000 characters ||
|| Events per request for `Event.get` | 1–1000 (default 100) ||
|#

## API Revisions and Compatibility

Bitrix24 cloud and on-premise versions may have different API revisions. To find out which revision is installed on a specific account, use [imbot.v2.Revision.get](./imbot.v2/revision-get.md).

New features, fixes, and breaking changes that affect integration compatibility are collected on the [Change Log for API imbot.v2](./change-log.md).

## Overview of Methods {#all-methods}

### Chatbots `imbot.v2`

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute methods: owner of the registered bot

**Bots**

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md) | Registers a new bot ||
|| [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md) | Updates bot properties ||
|| [imbot.v2.Bot.get](./imbot.v2/bots/bot-get.md) | Returns information about the bot ||
|| [imbot.v2.Bot.list](./imbot.v2/bots/bot-list.md) | Returns a list of the application's bots ||
|| [imbot.v2.Bot.unregister](./imbot.v2/bots/bot-unregister.md) | Deletes the bot ||
|#

**Chats**

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Chat.add](./imbot.v2/chats/chat-add.md) | Creates a group chat ||
|| [imbot.v2.Chat.get](./imbot.v2/chats/chat-get.md) | Returns information about the chat ||
|| [imbot.v2.Chat.update](./imbot.v2/chats/chat-update.md) | Updates chat properties ||
|| [imbot.v2.Chat.User.add](./imbot.v2/chats/chat-user-add.md) | Adds participants to the chat ||
|| [imbot.v2.Chat.User.delete](./imbot.v2/chats/chat-user-delete.md) | Removes a participant from the chat ||
|| [imbot.v2.Chat.User.list](./imbot.v2/chats/chat-user-list.md) | Returns a list of chat participants ||
|| [imbot.v2.Chat.leave](./imbot.v2/chats/chat-leave.md) | Leaves the chat ||
|| [imbot.v2.Chat.Manager.add](./imbot.v2/chats/chat-manager-add.md) | Adds chat managers ||
|| [imbot.v2.Chat.Manager.delete](./imbot.v2/chats/chat-manager-delete.md) | Removes chat managers ||
|| [imbot.v2.Chat.setOwner](./imbot.v2/chats/chat-set-owner.md) | Assigns a new owner to the chat ||
|#

**Messages**

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md) | Sends a message in the chat ||
|| [imbot.v2.Chat.Message.update](./imbot.v2/messages/chat-message-update.md) | Updates the bot's message ||
|| [imbot.v2.Chat.Message.delete](./imbot.v2/messages/chat-message-delete.md) | Deletes a message ||
|| [imbot.v2.Chat.Message.read](./imbot.v2/messages/chat-message-read.md) | Marks messages as read ||
|| [imbot.v2.Chat.Message.get](./imbot.v2/messages/chat-message-get.md) | Returns a message by ID. Only for `supervisor` and `personal` ||
|| [imbot.v2.Chat.Message.getContext](./imbot.v2/messages/chat-message-get-context.md) | Returns the message window around the specified one. Only for `supervisor` and `personal` ||
|| [imbot.v2.Chat.Message.Reaction.add](./imbot.v2/messages/chat-message-reaction-add.md) | Adds a reaction to a message ||
|| [imbot.v2.Chat.Message.Reaction.delete](./imbot.v2/messages/chat-message-reaction-delete.md) | Removes a reaction from a message ||
|#

**Commands**

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Command.register](./imbot.v2/commands/command-register.md) | Registers a slash command ||
|| [imbot.v2.Command.update](./imbot.v2/commands/command-update.md) | Updates the command ||
|| [imbot.v2.Command.list](./imbot.v2/commands/command-list.md) | Returns a list of the bot's commands ||
|| [imbot.v2.Command.unregister](./imbot.v2/commands/command-unregister.md) | Deletes the command ||
|| [imbot.v2.Command.answer](./imbot.v2/commands/command-answer.md) | Responds to a command call ||
|#

**Interface**

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Chat.InputAction.notify](./imbot.v2/ui/chat-input-action-notify.md) | Shows the bot's action indicator ||
|| [imbot.v2.Chat.TextField.enabled](./imbot.v2/ui/chat-text-field-enabled.md) | Enables or disables the text input field ||
|#

**Events**

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Event.get](./imbot.v2/events/event-get.md) | Returns bot events in polling mode ||
|#

A separate reference for event formats: [imbot.v2/events/events.md](./imbot.v2/events/events.md).

**Files**

#| 
|| **Method** | **Description** ||
|| [imbot.v2.File.upload](./imbot.v2/files/file-upload.md) | Uploads a file to the chat ||
|| [imbot.v2.File.download](./imbot.v2/files/file-download.md) | Returns a link to download the file ||
|#

**API Revisions**

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Revision.get](./imbot.v2/revision-get.md) | Returns API and client protocol revision numbers ||
|#

### Working with Chat `im.v2`

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute methods: user or application with access to the messenger

**Events**

#| 
|| **Method** | **Description** ||
|| [im.v2.Event.subscribe](./im.v2/events/event-subscribe.md) | Subscribes to event recording ||
|| [im.v2.Event.unsubscribe](./im.v2/events/event-unsubscribe.md) | Unsubscribes from event recording ||
|| [im.v2.Event.get](./im.v2/events/event-get.md) | Returns events in polling mode ||
|#

**Files**

#| 
|| **Method** | **Description** ||
|| [im.v2.File.upload](./im.v2/files/file-upload.md) | Uploads a file to the chat ||
|| [im.v2.File.download](./im.v2/files/file-download.md) | Returns a link to download the file ||
|#

## Continue Your Learning

- [Change Log for API imbot.v2](./change-log.md)
- [{#T}](./quick-start.md)
- [{#T}](./entities.md)
- [{#T}](./migration.md)
- [{#T}](./imbot.v2/events/events.md)
- [{#T}](./im.v2/index.md)