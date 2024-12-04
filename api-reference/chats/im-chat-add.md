# Create Chat im.chat.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- links to yet-to-be-created pages are not included

{% endnote %}

{% endif %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.add` creates a chat.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **TYPE**
[`unknown`](../data-types.md) | `CHAT` | Type of chat OPEN \| CHAT (OPEN - open for joining chat, CHAT - regular invitation-only chat, default is CHAT) | 18 ||
|| **TITLE**
[`unknown`](../data-types.md) | `My new private chat` | Chat title | 18 ||
|| **DESCRIPTION**
[`unknown`](../data-types.md) | `Very important chat` | Chat description | 18 ||
|| **COLOR**
[`unknown`](../data-types.md) | `PINK` | Chat color for mobile app: RED, GREEN, MINT, LIGHT_BLUE, DARK_BLUE, PURPLE, AQUA, PINK, LIME, BROWN, AZURE, KHAKI, SAND, MARENGO, GRAY, GRAPHITE | 18 ||
|| **MESSAGE**
[`unknown`](../data-types.md) | `Welcome to the chat` | First welcome message in the chat | 18 ||
|| **USERS^*^**
[`unknown`](../data-types.md) | `Array(1,2)` | Chat participants | 18 ||
|| **AVATAR**
[`unknown`](../data-types.md) | `base64 image` | Chat avatar in base64 format | 18 ||
|| **ENTITY_TYPE**
[`unknown`](../data-types.md) | `CHAT` | Entity identifier, can be used for searching by this field and for easy context identification in event handlers [ONIMBOTMESSAGEADD](.), [ONIMBOTMESSAGEUPDATE](.), [ONIMBOTMESSAGEDELETE](.) | 18 ||
|| **ENTITY_ID**
[`unknown`](../data-types.md) | `13` | Numeric entity identifier, can be used for searching the chat and for easy context identification in event handlers [ONIMBOTMESSAGEADD](.), [ONIMBOTMESSAGEUPDATE](.), [ONIMBOTMESSAGEDELETE](.) | 18 ||
|| **OWNER_ID**
[`unknown`](../data-types.md) | `39` | Identifier of the chat owner. It can be omitted; the owner will be the one making the request. | 18 ||
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

{% include [Example Notes](../../_includes/examples.md) %}

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
    "error_description": "Chat participants were not provided"
}
```

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **USERS_EMPTY** | Chat participants were not provided ||
|| **WRONG_REQUEST** | Something went wrong ||
|#