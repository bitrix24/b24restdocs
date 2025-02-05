# Find Chats by Names im.search.chat.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.chat.list` performs a search for chats.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **FIND^*^**
[`unknown`](../../data-types.md) | `Mint` | Search phrase | 19 ||
|| **OFFSET**
[`unknown`](../../data-types.md) | `0` | User sample offset | 19 ||
|| **LIMIT**
[`unknown`](../../data-types.md) | `10` | User sample limit | 19 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

- The search is conducted across the following fields: **Title**, **First Name**, and **Last Name** of chat participants.
- The method supports standard pagination of the Bitrix24 Rest API, but in addition, it allows navigation using the `OFFSET` and `LIMIT` parameters.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.search.chat.list',
        {
            FIND: 'Mint'
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log('users', result.data());
                console.log('total', result.total());
            }
        }
    );
    ```

- PHP

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.search.chat.list',
        Array(
            'FIND' => 'Mint'
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Examples Notes](../../../_includes/examples.md) %}

## Successful Response

```json
{    
    "result": {
        21191: {
            "id": 21191,
            "title": "Mint Chat #3",
            "owner": 2,
            "extranet": false,
            "avatar": "",
            "color": "#4ba984",
            "type": "chat",
            "entity_type": "",
            "entity_data_1": "",
            "entity_data_2": "",
            "entity_data_3": "",
            "date_create": "2017-10-14T12:15:32+02:00",
            "message_type": "C"
        }
    },
    "total": 1
}    
```

### Key Descriptions

- `id` – chat identifier
- `title` – chat title
- `owner` – identifier of the user who owns the chat
- `color` – chat color in hex format
- `avatar` – link to the avatar (if empty, the avatar is not set)
- `type` – type of chat (group chat, call chat, open line chat, etc.)
- `entity_type` – external code for the chat – type
- `entity_id` – external code for the chat – identifier
- `entity_data_1` – external data for the chat
- `entity_data_2` – external data for the chat
- `entity_data_3` – external data for the chat
- `date_create` – chat creation date in ATOM format
- `message_type` – type of chat messages

## Error Response

```json
{
    "error": "FIND_SHORT",
    "error_description": "Too short a search phrase."
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **FIND_SHORT** | The search phrase is too short; searching requires at least three characters. ||
|#