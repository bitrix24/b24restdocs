# Create a chat on behalf of the chatbot imbot.chat.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.add` creates a chat on behalf of the chatbot.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **TYPE**
[`unknown`](../../data-types.md) | `'CHAT'` | OPEN - open chat for joining, CHAT – regular invitation-only chat, default is CHAT | ||
|| **TITLE**
[`unknown`](../../data-types.md) | `'My new private chat'` | Title | ||
|| **DESCRIPTION**
[`unknown`](../../data-types.md) | `'Very important events'` | Description | ||
|| **COLOR**
[`unknown`](../../data-types.md) | `'PINK'` | Color for mobile app - RED, GREEN, MINT, LIGHT_BLUE, DARK_BLUE, PURPLE, AQUA, PINK, LIME, BROWN, AZURE, KHAKI, SAND, MARENGO, GRAY, GRAPHITE | ||
|| **MESSAGE**
[`unknown`](../../data-types.md) | `'Welcome!'` | First welcome message in the chat | ||
|| **USERS^*^**
[`unknown`](../../data-types.md) | `Array(1,2)` | Participants | ||
|| **AVATAR**
[`unknown`](../../data-types.md) | `'/* base64 image */'` | Avatar in base64 format | ||
|| **ENTITY_TYPE**
[`unknown`](../../data-types.md) | `'CHAT'` | Identifier of a custom entity (e.g., CHAT, CRM, OPENLINES, CALL, etc.), can be used to search for the chat and to easily determine the context in event handlers ONIMBOTMESSAGEADD, ONIMBOTMESSAGEUPDATE, ONIMBOTMESSAGEDELETE | ||
|| **ENTITY_ID**
[`unknown`](../../data-types.md) | `13` | Numeric identifier of the entity, can be used to search for the chat and to easily determine the context in event handlers ONIMBOTMESSAGEADD, ONIMBOTMESSAGEUPDATE, ONIMBOTMESSAGEDELETE | ||
|| **OWNER_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the owner. Can be omitted if you are creating a chat for the intended user | ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the bot making the request. Can be omitted if there is only one | ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'imbot.chat.add',
    Array(
        'TYPE' => 'CHAT',
        'TITLE' => 'My new private chat',
        'DESCRIPTION' => 'Very important events',
        'COLOR' => 'PINK',
        'MESSAGE' => 'Welcome!',
        'USERS' => Array(1,2),
        'AVATAR' => '/* base64 image */',
        'ENTITY_TYPE' => 'CHAT',
        'ENTITY_ID' => 13,
        'OWNER_ID' => 39,
        'BOT_ID' => 39,
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Example notes](../../../_includes/examples.md) %}

## Success response

Numeric identifier `CHAT_ID`.

## Error response

error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **USERS_EMPTY** | No chat participants provided. ||
|| **WRONG_REQUEST** | Something went wrong. ||
|#