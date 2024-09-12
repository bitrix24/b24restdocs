# User Block USER

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

![User Block](./_images/user.png)

`USER` - displays a block with the user's avatar and name.

The fields **AVATAR** (avatar) and **LINK** (link) are not mandatory.

## Example

{% list tabs %}

- JS

    ```js
    {
        USER: {
            NAME: "John Smith",
            AVATAR: "https://files.shelenkov.com/bitrix/images/avatar.png",
            LINK: "https://shelenkov.com"
        }
    },
    ```

- PHP

    ```php
    Array(
        "USER" => Array(
            "NAME" => "John Smith",
            "AVATAR" => "https://files.shelenkov.com/bitrix/images/avatar.png",
            "LINK" => "https://shelenkov.com/",
        )
    ),
    ```

{% endlist %}

{% include [Footnote about examples](../../../../../_includes/examples.md) %}

Instead of the key **LINK**, you can also use links to entities:
- `CHAT_ID` - to specify a link to a chat;
- `BOT_ID` - to specify a link to a bot;
- `USER_ID` - to specify a link to a user.