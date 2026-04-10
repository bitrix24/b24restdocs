# User Block USER

The `USER` block displays the user's card within the attachment: name, avatar, and a link for navigation.

![User Block](./_images/user.png){width=420}

## Block Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **NAME*** 
[`string`](../../../../../../data-types.md) | The name displayed in the block ||
|| **AVATAR** 
[`string`](../../../../../../data-types.md) | Avatar URL. Absolute URLs (`http://`, `https://`) and relative paths from the root of Bitrix are allowed ||
|| **LINK** 
[`string`](../../../../../../data-types.md) | URL for navigation when clicking on the block. It is preferable to use `USER_ID`, `CHAT_ID`, `BOT_ID` for navigation within the messenger ||
|| **USER_ID** 
[`integer`](../../../../../../data-types.md) | Link to the Bitrix user ||
|| **CHAT_ID** 
[`integer`](../../../../../../data-types.md) | Link to the Bitrix chat ||
|| **BOT_ID** 
[`integer`](../../../../../../data-types.md) | Link to the Bitrix chatbot ||
|| **NETWORK_ID** 
[`string`](../../../../../../data-types.md) | Link to the Bitrix24 Network user ||
|| **AVATAR_TYPE** 
[`string`](../../../../../../data-types.md) | Type of avatar display. Allowed values: `USER`, `CHAT`, `BOT` ||
|#

## Example

{% include [Example Note](../../../../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    {
        USER: {
            NAME: 'John Smith',
            AVATAR: 'https://files.shelenkov.com/bitrix/images/avatar.png',
            LINK: 'https://shelenkov.com'
        }
    }
    ```

- PHP

    ```php
    [
        'USER' => [
            'NAME' => 'John Smith',
            'AVATAR' => 'https://files.shelenkov.com/bitrix/images/avatar.png',
            'LINK' => 'https://shelenkov.com'
        ]
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./links.md)
- [{#T}](./grid.md)