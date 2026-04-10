# Change Log for API imbot.v2

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The change log for API `imbot.v2`: new features, fixes, and changes with breaking changes. Entries are listed from newest to oldest.

## Entry Prefixes

- **BOT-NEW**: new functionality
- **BOT-FIX**: bug fix or incorrect behavior
- **BOT-BC**: breaking change. The old format is supported for **6 months** from the date of publication, after which it may be discontinued without notice.

Code format: `BOT-{TYPE}-{MMDD}-{N}`, where `MMDD` is the publication date, and `N` is a sequential number for that date.

## REST Revision 34 {#rest-revision-34}

> Publication Date: 04/08/2026

### BOT-NEW-0408-1: Documentation, General Error Codes for BotController

Descriptions of basic error codes that may occur at the `BotController` level have been added to all methods of `imbot.v2`:

- `BOT_TOKEN_NOT_SPECIFIED`: `botToken` not provided during webhook authorization
- `BOT_ID_REQUIRED`: `botId` not provided
- `BOT_NOT_FOUND`: bot not found
- `BOT_OWNERSHIP_ERROR`: bot does not belong to the application

### BOT-NEW-0408-2: Documentation, Additional Materials

Links to documentation on advanced messaging capabilities have been added to the [Quick Start](./quick-start.md): text formatting, `Attach` attachments, and `Keyboard` keyboards.

### BOT-FIX-0408-3: Command.list, Fixed Localization Display

**Before**

[imbot.v2.Command.list](./imbot.v2/commands/command-list.md) returned empty `title` and `params` for commands registered via REST, including [imbot.v2.Command.register](./imbot.v2/commands/command-register.md) and [imbot.v2.Command.update](./imbot.v2/commands/command-update.md). Localizations were saved to the database correctly but were not read due to an error in mapping identifiers.

**After**

`title` and `params` are now correctly returned in the current account's language with a fallback to the default language.

**Affected Method:** [imbot.v2.Command.list](./imbot.v2/commands/command-list.md)

### BOT-FIX-0408-4: Bot.register / Bot.update, Normalization of backgroundId

**Before**

`backgroundId` did not pass validation through the enum of allowed values. Invalid values, such as typos in case or non-existent IDs, were saved to the database as is.

**After**

`backgroundId` is normalized through `Background::normalizeBackgroundId()`. Invalid values are converted to `null`. Allowed values: `azure`, `mint`, `steel`, `slate`, `teal`, `cornflower`, `sky`, `peach`, `frost`.

**Affected Methods:** [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md), [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md)

### BOT-FIX-0408-5: Chat.TextField.enabled, Support for P2P Chats

**Before**

[imbot.v2.Chat.TextField.enabled](./imbot.v2/ui/chat-text-field-enabled.md) returned `ACCESS_DENIED` in private P2P chats with the bot, even though this is the primary use case for the method.

**After**

The method checks the bot's membership in the chat instead of the edit permission. It works in any chat where the bot is a participant.

**Affected Method:** [imbot.v2.Chat.TextField.enabled](./imbot.v2/ui/chat-text-field-enabled.md)

### BOT-FIX-0408-6: Error Code BOT_TOKEN_NOT_SPECIFIED

**Before**

When calling methods via webhook without `botToken`, the error `CLIENT_ID_NOT_SPECIFIED` was returned.

**After**

`BOT_TOKEN_NOT_SPECIFIED` is returned, which corresponds to the actual reason for the error.

**Affected Methods:** [imbot.v2.Bot.register](./imbot.v2/bots/bot-register.md), [imbot.v2.Bot.list](./imbot.v2/bots/bot-list.md), [imbot.v2.Bot.get](./imbot.v2/bots/bot-get.md), [imbot.v2.Bot.update](./imbot.v2/bots/bot-update.md), [imbot.v2.Bot.unregister](./imbot.v2/bots/bot-unregister.md)

## REST Revision 33 {#rest-revision-33}

> Publication Date: 04/02/2026

### BOT-NEW-0402-1: New Method Revision.get {#revision-get-new-method}

The method [imbot.v2.Revision.get](./imbot.v2/revision-get.md) has been added to retrieve API revision numbers. It allows determining the compatibility of Bitrix24 with specific methods and fields and does not require `botId` and `botToken`.

### BOT-FIX-0402-2: Chat.Message.send, Normalization of Boolean Fields

**Before**

When passing `fields.system`, `fields.urlPreview`, `fields.skipConnector`, `fields.silentConnector` as JSON booleans, i.e., `true` or `false`, the values were not converted to `Y` or `N` and could be ignored.

**After**

All four boolean fields are now correctly normalized. Allowed values are `true`, `false`, `"Y"`, `"N"`.

**Affected Method:** [imbot.v2.Chat.Message.send](./imbot.v2/messages/chat-message-send.md)

---

## BOT-BC-0330-1: Command.register, Parameters Moved to Fields {#command-register-fields}

> Publication Date: 03/30/2026 | Support for the old format until: 09/30/2026

**Before (legacy)**

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

**After**

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

If `fields` is provided, the new format is used. If `fields` is absent, the legacy format is used. The old flat format is supported until 09/30/2026.

**Affected Method:** [imbot.v2.Command.register](./imbot.v2/commands/command-register.md)

---

## BOT-BC-0325-1: Event.get, Pagination via nextOffset {#event-get-next-offset}

> Publication Date: 03/25/2026 | Support for the old format until: 09/25/2026

**Before**

The response contained the `lastEventId` field, and the client had to compute the next `offset` as `lastEventId + 1`.

**After**

The response now contains the `nextOffset` field. To retrieve the next batch, pass this value in the `offset` parameter.

```json
{
  "result": {
    "events": [],
    "nextOffset": 1002,
    "hasMore": false
  }
}
```

The next request:

```json
{
  "botId": 456,
  "botToken": "...",
  "offset": 1002
}
```

If `hasMore: false`, there are no new events, but `nextOffset` still needs to be saved for the next polling cycle.

**Affected Method:** [imbot.v2.Event.get](./imbot.v2/events/event-get.md)

---

## See Also

- [Quick Start](./quick-start.md): Getting started with the bot platform
- [Migration from imbot to imbot.v2](./migration.md): Transition from the legacy API