# Passing Context to the Bot When Opening a Chat

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The mechanism for passing the initial context to the bot when opening a chat. The user follows a special link from an external site, via a QR code, or a deep link from the application, the messenger opens a dialog with the bot, and the bot immediately receives arbitrary JSON data—without any additional steps.

With this data, the bot instantly understands why the user has arrived: it opens the required detail form, launches the adaptation workflow, issues a coupon, or starts the authorization procedure.

## How the Mechanism Works

1. The user follows the link `?IM_DIALOG={dialogId}&BOT_CONTEXT={json}` or the native `bx` deep link.
2. The Bitrix24 web interface opens a chat with the bot.
3. After initializing the dialog, the client sends a request `im.v2.Chat.Bot.sendContext` with `{dialogId, context}`.
4. The platform forwards the context to the bot—through a PHP handler (modular bots) **and** through the event `ONIMBOTV2CONTEXTGET` (REST bots).
5. The bot responds with its own method calls: sends a message, displays a keyboard, and so on.

{% note info "" %}

The `BOT_CONTEXT` parameter is automatically removed from the browser's address bar via `history.replaceState`—after the chat is opened, the context token is no longer in the history.

{% endnote %}

## Link Formats

The context can be passed in two ways: through the browser or via a `bx` deep link for desktop and mobile applications. Both methods deliver the same data, so the logic for processing in the bot does not depend on the chosen method.

It is not possible to determine from the integration side whether the user has the application installed. Recommended approaches in the UI:

- **Two buttons**—“Open in App” (`bx` link) and “Open in Browser” (http link), allowing the user to choose.
- **Landing page**—automatically attempts to open the `bx` link. If the application does not intercept the transition within the allotted time, it shows a fallback button with the http link.

### Web: GET Parameters

Works on any Bitrix24 page:

```
https://{portal}/online/?IM_DIALOG={dialogId}&BOT_CONTEXT={urlEncodedJson}
```

#| 
|| **Parameter** | **Description** ||
|| `IM_DIALOG` | `dialogId` — numeric ID of the user-bot ||
|| `BOT_CONTEXT` | JSON of arbitrary structure in URL encoding ||
|#

Example link:

```
https://portal.bitrix24.com/online/?IM_DIALOG=1459503&BOT_CONTEXT=%7B%22pairCode%22%3A%2213bd812901b295c50adbc%22%7D
```

### Application: bx Deep Link

Deep link for the desktop and mobile Bitrix24 application. Opens a chat with the bot directly through the `bx://` protocol handler:

```
bx://v2/{portal}/botContext/dialogId/{dialogId}/context/{urlEncodedJson}
```

#| 
|| **Segment** | **Description** ||
|| `v2` | Version of the application protocol, currently always `v2` ||
|| `{portal}` | Bitrix24 domain ||
|| `botContext` | Type of deep link, fixed value ||
|| `{dialogId}` | Numeric ID of the user-bot ||
|| `{urlEncodedJson}` | `encodeURIComponent(JSON.stringify(context))` ||
|#

Example:

```
bx://v2/portal.bitrix24.com/botContext/dialogId/1459503/context/%7B%22pairCode%22%3A%2213bd812901b295c50adbc%22%7D
```

## JS API

If the chat is opened programmatically from within the Bitrix24 page, use the JS API:

```js
import { Messenger } from 'im.public';

Messenger.openChatWithBotContext(dialogId, { pairCode: '13bd812901b295c50adbc' });
```

#| 
|| **Parameter** | **Type** | **Description** ||
|| `dialogId` | `string` | Identifier of the bot (numeric ID) ||
|| `context` | `object` | Arbitrary JSON object ||
|#

The method creates an internal context service, subscribes to `onDialogInited`, and after initializing the dialog, sends a request `im.v2.Chat.Bot.sendContext`.

## Event ONIMBOTV2CONTEXTGET

The main point for processing context for the bot. The event is delivered in the same mode that was chosen when registering the bot via [imbot.v2.Bot.register](./bots/bot-register.md)—no separate subscription is required.

- **Fetch mode** (`eventMode: "fetch"`, by default): the event is retrieved via [imbot.v2.Event.get](./events/event-get.md).
- **Webhook mode** (`eventMode: "webhook"`): the event arrives as a request to `webhookUrl`.

Complete descriptions of all fields, delivery modes, and data examples can be found in the [event reference](./events/events.md#onimbotv2contextget).

### Event Fields

#| 
|| **Field** | **Type** | **Description** ||
|| `bot` | `object` | Recipient bot. Format depends on the mode: in fetch — full bot object, in webhook — `{id, code, auth}` ||
|| `dialogId` | `string` | Identifier of the dialog, for example `"chat5"` ||
|| `context` | `object` | Arbitrary JSON passed when opening the chat ||
|| `chat` | `object` | Chat object ||
|| `user` | `object` | User who initiated the opening ||
|| `language` | `string` | Language of the bot ||
|#

{% note warning "" %}

`user.id` in the event comes as **number** (not string) in both fetch and webhook modes. Use `user.id` directly for matching with stored data.

{% endnote %}

The handler's response does not affect the scenario: the bot responds with its own method calls, for example, [imbot.v2.Chat.Message.send](./messages/chat-message-send.md).

### Example: Webhook Context Handler

```php
// webhook_handler.php
$data = json_decode(file_get_contents('php://input'), true);

if (($data['event'] ?? '') === 'ONIMBOTV2CONTEXTGET') {
    $eventData = $data['data'] ?? [];
    $context   = $eventData['context'] ?? [];
    $dialogId  = $eventData['chat']['dialogId'] ?? '';
    $botId     = (int)($eventData['bot']['id'] ?? 0);

    if (($context['action'] ?? null) === 'openTask') {
        $taskId = (int)($context['taskId'] ?? 0);
        CRest::call('imbot.v2.Chat.Message.send', [
            'botId'    => $botId,
            'dialogId' => $dialogId,
            'fields'   => ['message' => "Opening task #{$taskId}"],
        ]);
    }

    http_response_code(200);
    echo json_encode(['status' => 'ok']);
}
```

## Use Cases

### Launch via Deep Link

The most common case: the link carries a short startup context, and the bot immediately "knows why the user has come" and launches the required scenario. Identity binding and confirmation are not required.

#| 
|| **Scenario** | **Example `context`** | **What the bot does** ||
|| Deep link to content | `{"action":"openTask","taskId":456}` | Opens the detail form, launches the scenario ||
|| Referral / invitation | `{"ref":"u12345"}` | Binds the new user to the inviter ||
|| Adaptation | `{"onboard":"teamX"}` | Pre-fills the team, role, or organization ||
|| Activation / redemption | `{"claim":"GIFT-7H2K"}` | Issues a coupon / gift / order ||
|| Offline → chat (QR) | `{"table":42}` | Context for support or order from offline ||
|#

**Two ways to pass data:**

- **Embedded data**—directly in the link. Suitable for small and non-confidential data.
- **Token link**—a short token is placed in `BOT_CONTEXT`, and the real data is restored from it on your server. Suitable for large or confidential data.

### Binding and Authorization

The same deep link, but the parameter is an authorization or binding token, and the result is the binding of a verified identity: session creation or issuance of a personal key. The server lifecycle of `pairCode` and mandatory confirmation in the bot are added on top of the basic scenario.

Typical scenario:

1. The external server generates `pairCode` and returns a link to the bot to the browser.
2. The user follows the link—the bot receives `ONIMBOTV2CONTEXTGET` with the verified `user.id`.
3. The bot does not perform the binding immediately but shows a confirmation detail form with information about the initiator.
4. The user clicks "Confirm"—a separate event `ONIMBOTV2COMMANDADD` arrives.
5. Only after the button is pressed does the server perform the binding (creates a session, issues a key).

## Security

{% note warning "" %}

The event `ONIMBOTV2CONTEXTGET` should not immediately perform actions with side effects: creating a session, binding an account, issuing a key. The event only initiates the scenario—its completion requires explicit user confirmation in the bot.

{% endnote %}

**Why confirmation is mandatory.** Without it, `CONTEXTGET` is an unnoticed automatic binding. An attacker embeds an image on their page with a URL like `https://victim-address.com/online/?...BOT_CONTEXT={attacker_pairCode}`—the victim's browser silently calls `CONTEXTGET`, and the victim's identity is bound to the attacker's code. An explicit confirmation detail form in the bot with the name or IP/time of the initiator allows the user to see the foreign request and reject it.

**Recommendations for implementing binding:**

- **Two independent secrets.** `pairCode` is passed via the link, while the result (session or key) is received by the browser through a separate `httpOnly` cookie. An intercepted `pairCode` does not reveal the binding result.
- **Atomic state change.** Upon receiving `CONTEXTGET`, change the record state to `awaiting_confirm` in one atomic operation and store `user.id` in it. When the button is pressed, change the state from `awaiting_confirm → consumed` also in one atomic operation and only if the `user.id` of the button presser matches the stored one.
- **String escaping.** Escape user data (name, email) before inserting it into the bot's message BB markup.
- **Initiator of the request** should be displayed in the confirmation detail form: name/email or IP/browser/time.

### Lifecycle of pairCode

#| 
|| **Parameter** | **Recommendation** ||
|| Format | 64 hexadecimal characters (256 bits), cryptographically random ||
|| Time to live (TTL) | ~10–15 minutes ||
|| States | `pending → awaiting_confirm → confirmed \| declined \| expired` ||
|| One-time use | Change state in one atomic operation: `SELECT … FOR UPDATE` or `UPDATE … WHERE status=… RETURNING` ||
|| Protection against brute force | Unknown, expired, or foreign code → `200` without data leakage; delete expired records on a schedule ||
|#

## Examples

### Link from an External Site

```html
<a href="https://portal.bitrix24.com/online/?IM_DIALOG=1234&BOT_CONTEXT=%7B%22action%22%3A%22openTask%22%2C%22taskId%22%3A456%7D">
    Ask the bot a question about the task
</a>
```

The bot will receive in `context`: `{"action": "openTask", "taskId": 456}`.

### Call from JS Code of the Application

```js
import { Messenger } from 'im.public';

Messenger.openChatWithBotContext('1234', {
    action: 'openTask',
    taskId: 456,
    source: 'taskCard',
});
```

### Event ONIMBOTV2CONTEXTGET (fetch mode)

```json
{
    "type": "ONIMBOTV2CONTEXTGET",
    "data": {
        "bot": { "id": 456, "code": "my_bot", "eventMode": "fetch" },
        "dialogId": "chat5",
        "context": { "action": "openTask", "taskId": 456 },
        "chat": { "id": 5, "dialogId": "chat5", "type": "chat" },
        "user": { "id": 274, "name": "John Smith" },
        "language": "en"
    }
}
```

### Binding Event with pairCode

```json
{
    "type": "ONIMBOTV2CONTEXTGET",
    "data": {
        "chat": { "dialogId": "274" },
        "user": { "id": 274, "name": "John Smith" },
        "context": { "pairCode": "e5142e4ad735..." }
    }
}
```

`user.id` here is `274`—this is a `number`, not a string. Use it for binding after confirmation.

## Continue Learning

- [{#T}](./events/events.md) — complete description of all events, including `ONIMBOTV2CONTEXTGET` and `ONIMBOTV2COMMANDADD`
- [{#T}](../quick-start.md) — step-by-step start with bot registration and the first message
- [{#T}](./bots/bot-register.md) — bot registration parameters, `eventMode`, and `webhookUrl`
- [{#T}](./events/event-get.md) — retrieving events in fetch mode
- [{#T}](./messages/chat-message-send.md) — sending a response message from the bot