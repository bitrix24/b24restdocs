# Create Chat im.chat.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- links to pages that have not been created yet are not provided

{% endnote %}

{% endif %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.add` creates a chat.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **TYPE**
[`unknown`](../data-types.md) | `CHAT` | Type of chat OPEN \| CHAT (OPEN - open for joining chat, CHAT - regular invite-only chat, default is CHAT) | 18 ||
|| **TITLE**
[`unknown`](../data-types.md) | `My new private chat` | Title of the chat | 18 ||
|| **DESCRIPTION**
[`unknown`](../data-types.md) | `Very important chat` | Description of the chat | 18 ||
|| **COLOR**
[`unknown`](../data-types.md) | `PINK` | Color of the chat for the mobile app: RED, GREEN, MINT, LIGHT_BLUE, DARK_BLUE, PURPLE, AQUA, PINK, LIME, BROWN, AZURE, KHAKI, SAND, MARENGO, GRAY, GRAPHITE | 18 ||
|| **MESSAGE**
[`unknown`](../data-types.md) | `Welcome to the chat` | First welcome message in the chat | 18 ||
|| **USERS^*^**
[`unknown`](../data-types.md) | `Array(1,2)` | Participants of the chat | 18 ||
|| **AVATAR**
[`unknown`](../data-types.md) | `base64 image` | Avatar of the chat in base64 format | 18 ||
|| **ENTITY_TYPE**
[`unknown`](../data-types.md) | `CHAT` | Entity identifier, can be used for searching by this field and for easy context identification in event handlers [ONIMBOTMESSAGEADD](../chat-bots/messages/events/on-imbot-message-add.md), [ONIMBOTMESSAGEUPDATE](../chat-bots/messages/events/on-imbot-message-update.md), [ONIMBOTMESSAGEDELETE](../chat-bots/messages/events/on-imbot-message-delete.md) | 18 ||
|| **ENTITY_ID**
[`unknown`](../data-types.md) | `13` | Numeric identifier of the entity, can be used for searching the chat and for easy context identification in event handlers [ONIMBOTMESSAGEADD](../chat-bots/messages/events/on-imbot-message-add.md), [ONIMBOTMESSAGEUPDATE](../chat-bots/messages/events/on-imbot-message-update.md), [ONIMBOTMESSAGEDELETE](../chat-bots/messages/events/on-imbot-message-delete.md) | 18 ||
|| **OWNER_ID**
[`unknown`](../data-types.md) | `39` | Identifier of the chat owner. It can be omitted; the owner will be the one from whom the request is made. | 18 ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](./_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'im.chat.add',
        Array(
            'TYPE' => 'CHAT',
            'TITLE' => 'My new private chat',
            'DESCRIPTION' => 'Very important chat',
            'COLOR' => 'PINK',
            'MESSAGE' => 'Welcome to the chat',
            'USERS' => Array(1,2),
            'AVATAR' => 'base64 image',
            'ENTITY_TYPE' => 'CHAT',
            'ENTITY_ID' => 13,
            'OWNER_ID' => 39,
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

{% include [Examples Notes](../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": 123
}
```

## Response on Error

```json
{
    "error": "USERS_EMPTY",
    "error_description": "Participants of the chat are not provided"
}
```

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **USERS_EMPTY** | Participants of the chat are not provided ||
|| **WRONG_REQUEST** | Something went wrong ||
|#