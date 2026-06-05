# API Change Log for imbot.v2

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Change history for the API `imbot.v2`: new features, fixes, and changes with breaking changes. Entries are listed from newest to oldest.

## Record Prefixes

- **BOT-NEW**: new functionality
- **BOT-FIX**: bug fix or incorrect behavior
- **BOT-BC**: change with breaking compatibility. The old format is supported for **6 months** from the date of publication of the change, after which it may be discontinued without notice.

Code format: `BOT-{TYPE}-{MMDD}-{N}`, where `MMDD` is the publication date, and `N` is a sequential number for that date.

## REST Revision 36 {#rest-revision-36}

> Publication date: 06.05.2026

### BOT-BC-0506-1: Bot.register, upcoming botToken length limit {#bot-bc-0506-1}

> Limit application: from 06.08.2026 (in three months)

**Currently (temporarily)**

[imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md) accepts `botToken` of any length — for compatibility with integrations that previously registered bots with long tokens, such as 64-character SHA-256-hex.

**From 06.08.2026**

[imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md) will start rejecting `botToken` longer than **40 characters** with the error `BOT_TOKEN_INVALID_LENGTH`. The same limit already applies to token rotation via [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md) (`fields.botToken`) — see [#BOT-NEW-0501-2](#bot-new-0501-2).

**What integrators should do**

If you have bots registered with `botToken` longer than 40 characters, replace the token with one no longer than 40 characters within three months via `Bot.update {fields: {botToken: '...'}}`. The request authorization remains the old (long) token, while the new one is passed in `fields`. After 06.08.2026, re-registering a bot with a long token, for example, during state loss and `register-on-startup`, will be rejected. Already registered bots will continue to operate unchanged.

**Affected method:** [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md)

## REST Revision 35 {#rest-revision-35}

> Publication date: 01.05.2026

### BOT-FIX-0501-1: Bot.update / Bot.unregister, automatic cleanup of subscriptions to events ONIMBOTV2* {#bot-fix-0501-1}

**Previously**

When changing `webhookUrl` via [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md), switching `eventMode` from `webhook` to `fetch`, or deleting a bot via [imbot.v2.Bot.unregister](./imbot.v2/bots/bot-unregister.md), old subscriptions to events `ONIMBOTV2*` remained in the registry and continued to send events to the previously configured URL. After several URL changes, events were sent to all previous addresses simultaneously. Deleted webhook bots also continued to receive events until manually cleared via `event.unbind`.

**Now**

[imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md) and [imbot.v2.Bot.unregister](./imbot.v2/bots/bot-unregister.md) automatically bring the bot's subscriptions up to date:

- When changing `webhookUrl` — old subscriptions are deleted, new ones are created.
- When switching `eventMode: webhook → fetch` — all bot subscriptions are deleted.
- When `Bot.unregister` — all bot subscriptions are deleted for any mode.

Cleanup is performed based on the bot's `APPLICATION_TOKEN`. For OAuth applications with multiple bots, cleanup is skipped if the bot has active neighbors sharing the same `APPLICATION_TOKEN`.

**Impact on integrators**

If your code has a workaround with manual `event.unbind` after `Bot.update {eventMode: 'fetch'}` or `Bot.unregister` — it can be removed: after the fix, it does nothing.

**Affected methods:** [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md), [imbot.v2.Bot.unregister](./imbot.v2/bots/bot-unregister.md)

### BOT-NEW-0501-2: Bot.update, botToken rotation {#bot-new-0501-2}

The ability to change `botToken` via `fields.botToken` has been added to [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md). Request authorization is performed with the old token, while the new one is passed in `fields`.

After a successful rotation, all `ONIMBOTV2*` subscriptions are re-bound to the new `APPLICATION_TOKEN` (if the bot is in `webhook` mode), and the old token immediately loses access. Everything is done in a single transaction.

In case of token collision, the code `BOT_TOKEN_ROTATION_FAILED` is returned. **The length of `botToken` must not exceed 40 characters.**

**Affected method:** [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md)

### BOT-FIX-0501-3: Removal of the bot.appId field from method responses and events {#bot-fix-0501-3}

**Previously**

The field `bot.appId` was present in method responses and in the data of events `ONIMBOTV2*`.

**Now**

The field `bot.appId` has been removed from the responses of all methods and from all events `ONIMBOTV2*`.

**Impact on integrators**

If your code read `result.bot.appId` or `data.bot.appId` — the field is now absent. To check bot ownership, use `result.bot.code` or the mere fact of a successful method response (which is only available to the owner).

**Affected methods and events:** [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md), [imbot.v2.Bot.get](./imbot.v2/bots/bot-get.md), [imbot.v2.Bot.list](./imbot.v2/bots/bot-list.md), [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md), all events `ONIMBOTV2*`

## REST Revision 34 {#rest-revision-34}

> Publication date: 08.04.2026

### BOT-NEW-0408-1: Documentation, general error codes for BotController

Descriptions of basic error codes that may occur at the `BotController` level have been added to all `imbot.v2` methods:

- `BOT_TOKEN_NOT_SPECIFIED`: `botToken` not provided during webhook authorization
- `BOT_ID_REQUIRED`: `botId` not provided
- `BOT_NOT_FOUND`: bot not found
- `BOT_OWNERSHIP_ERROR`: bot does not belong to the application

### BOT-NEW-0408-2: Documentation, additional materials

Links to documentation on advanced messaging capabilities: text formatting, `Attach` attachments, `Keyboard` keyboards have been added to [Quick Start](./quick-start.md).

### BOT-FIX-0408-3: Command.list, fixed localization display

**Previously**

[imbot.v2.Command.list](./imbot.v2/commands/command-list.md) returned empty `title` and `params` for commands registered via REST, including [imbot.v2.Command.register](./imbot.v2/commands/command-register.md) and [imbot.v2.Command.update](./imbot.v2/commands/command-update.md). Localizations were correctly saved to the database but were not read due to an error in ID mapping.

**Now**

`title` and `params` are correctly returned in the interface language with a fallback to the default language.

**Affected method:** [imbot.v2.Command.list](./imbot.v2/commands/command-list.md)

### BOT-FIX-0408-4: Bot.register / Bot.update, normalization of backgroundId

**Previously**

`backgroundId` did not pass validation through the enum of allowed values. Invalid values, such as typos in case or non-existent IDs, were saved to the database as is.

**Now**

`backgroundId` is normalized through `Background::normalizeBackgroundId()`. Invalid values are set to `null`. Allowed values: `azure`, `mint`, `steel`, `slate`, `teal`, `cornflower`, `sky`, `peach`, `frost`.

**Affected methods:** [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md), [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md)

### BOT-FIX-0408-5: Chat.TextField.enabled, support for P2P chats

**Previously**

[imbot.v2.Chat.TextField.enabled](./imbot.v2/ui/chat-text-field-enabled.md) returned `ACCESS_DENIED` in private P2P chats with the bot, although this is the primary use case for the method.

**Now**

The method checks the bot's membership in the chat instead of the edit permission. It works in any chats where the bot is a participant.

**Affected method:** [imbot.v2.Chat.TextField.enabled](./imbot.v2/ui/chat-text-field-enabled.md)

### BOT-FIX-0408-6: Error code BOT_TOKEN_NOT_SPECIFIED

**Previously**

When calling methods via webhook without `botToken`, the error `CLIENT_ID_NOT_SPECIFIED` was returned.

**Now**

`BOT_TOKEN_NOT_SPECIFIED` is returned, which corresponds to the actual reason for the error.

**Affected methods:** [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md), [imbot.v2.Bot.list](./imbot.v2/bots/bot-list.md), [imbot.v2.Bot.get](./imbot.v2/bots/bot-get.md), [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md), [imbot.v2.Bot.unregister](./imbot.v2/bots/bot-unregister.md)

## REST Revision 33 {#rest-revision-33}

> Publication date: 02.04.2026

### BOT-NEW-0402-1: New method Revision.get {#revision-get-new-method}

The method [imbot.v2.Revision.get](./imbot.v2/revision-get.md) has been added to retrieve API revision numbers. It allows determining the compatibility of Bitrix24 with specific methods and fields and does not require `botId` and `botToken`.

### BOT-FIX-0402-2: Chat.Message.send, normalization of boolean fields

**Previously**

When passing `fields.system`, `fields.urlPreview`, `fields.skipConnector`, `fields.silentConnector` as JSON booleans, i.e., `true` or `false`, the values were not converted to `Y` or `N` and could be ignored.

**Now**

All four boolean fields are correctly normalized. Allowed values are `true`, `false`, `"Y"`, `"N"`.

**Affected method:** [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md)

---

## BOT-BC-0330-1: Command.register, parameters moved to fields {#command-register-fields}

> Publication date: 30.03.2026 | Support for the old format until: 30.09.2026

**Previously (legacy)**

Parameters were passed at the top level of the request:

```json
{
  "botId": 456,
  "botToken": "...",
  "command": "help",
  "title": {"en": "Help"},
  "hidden": false
}
```

**Now**

Command parameters are grouped in the `fields` object, as in other `imbot.v2` methods.

```json
{
  "botId": 456,
  "botToken": "...",
  "fields": {
    "command": "help",
    "title": {"en": "Help"},
    "hidden": false
  }
}
```

**Compatibility**

If `fields` is provided, the new format is used. If `fields` is absent, the legacy format is used. The old flat format is supported until 30.09.2026.

**Affected method:** [imbot.v2.Command.register](./imbot.v2/commands/command-register.md)

---

## BOT-BC-0325-1: Event.get, pagination through nextOffset {#event-get-next-offset}

> Publication date: 25.03.2026 | Support for the old format until: 25.09.2026

**Previously**

The response contained the field `lastEventId`, and the client had to compute the next `offset` as `lastEventId + 1`.

**Now**

The response contains the field `nextOffset`. To get the next batch, pass this value in the `offset` parameter.

```json
{
  "result": {
    "events": [],
    "nextOffset": 1002,
    "hasMore": false
  }
}
```

Next request:

```json
{
  "botId": 456,
  "botToken": "...",
  "offset": 1002
}
```

If `hasMore: false`, there are no new events, but `nextOffset` still needs to be saved for the next polling cycle.

**Affected method:** [imbot.v2.Event.get](./imbot.v2/events/event-get.md)

---

## See also

- [Quick Start](./quick-start.md): getting started with the bot platform
- [Migration from imbot to imbot.v2](./migration.md): transitioning from the legacy API