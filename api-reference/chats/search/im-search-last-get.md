# Get Search History im.search.last.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.last.get` returns a list of dialogs from the history of the last search.

This method was designed for the previous version of the chat. In the current M1 chat version, it works, but the results are not displayed in the interface.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **SKIP_OPENLINES**
[`string`](../../data-types.md) | Skip Open Channels chats.

Possible values:
- `Y` — yes
- `N` — no 

Default value — `N` ||
|| **SKIP_CHAT**
[`string`](../../data-types.md) | Skip group chats.

Possible values:
- `Y` — yes
- `N` — no 

Default value — `N` ||
|| **SKIP_DIALOG**
[`string`](../../data-types.md) | Skip personal dialogs. 

Possible values:
- `Y` — yes
- `N` — no 

Default value — `N` ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SKIP_OPENLINES":"N","SKIP_CHAT":"N","SKIP_DIALOG":"N"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.search.last.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SKIP_OPENLINES":"N","SKIP_CHAT":"N","SKIP_DIALOG":"N","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.search.last.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.search.last.get', {
        SKIP_OPENLINES: 'N',
        SKIP_CHAT: 'N',
        SKIP_DIALOG: 'N',
      });

      const { result } = response.getData();
      console.log(result);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.search.last.get',
            [
                'SKIP_OPENLINES' => 'N',
                'SKIP_CHAT' => 'N',
                'SKIP_DIALOG' => 'N',
            ]
        );

        $result = $response->getResponseData()->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            var_dump($result->data());
        }
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.search.last.get',
        {
            SKIP_OPENLINES: 'N',
            SKIP_CHAT: 'N',
            SKIP_DIALOG: 'N',
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.search.last.get',
        [
            'SKIP_OPENLINES' => 'N',
            'SKIP_CHAT' => 'N',
            'SKIP_DIALOG' => 'N',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        var_dump($result['result']);
    }
    ```
{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": [
        {
            "id": "chat1157",
            "type": "chat",
            "avatar": {
                "url": "",
                "color": "#ab7761"
            },
            "title": "Brown Chat #18",
            "chat": {
                "id": 1157,
                "name": "Brown Chat #18",
                "owner": 27,
                "extranet": false,
                "avatar": "",
                "color": "#ab7761",
                "type": "thread",
                "entity_type": "THREAD",
                "entity_id": "",
                "entity_data_1": "",
                "entity_data_2": "",
                "entity_data_3": "",
                "mute_list": [],
                "date_create": "2025-01-30T00:41:03+01:00",
                "message_type": "C"
            }
        },
        {
            "id": 103,
            "type": "user",
            "avatar": {
                "url": "https://example.bitrix24.com/upload/main/avatar.png",
                "color": "#4ba984"
            },
            "title": "Emily Smith",
            "user": {
                "id": 103,
                "active": true,
                "name": "Emily Smith",
                "first_name": "Emily",
                "last_name": "Smith",
                "work_position": "IT Department Head",
                "color": "#4ba984",
                "avatar": "https://example.bitrix24.com/upload/main/avatar.png",
                "avatar_hr": "https://example.bitrix24.com/upload/main/avatar.png",
                "gender": "F",
                "birthday": "08-03",
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "external_auth_id": "socservices",
                "status": "online",
                "idle": false,
                "last_activity_date": "2026-03-05T10:19:37+01:00",
                "mobile_last_date": false,
                "desktop_last_date": false,
                "absent": false,
                "departments": [1, 7],
                "phones": {
                    "personal_mobile": "81234567890",
                    "work_phone": "19123456789",
                    "inner_phone": "78"
                },
                "bot_data": null,
                "type": "user",
                "website": "",
                "email": "emily@example.com"
            }
        },
        ... // description for each chat, user
    ],
    "time": {
        "start": 1772695649,
        "finish": 1772695649.89509,
        "duration": 0.8950901031494141,
        "processing": 0,
        "date_start": "2026-03-05T10:27:29+01:00",
        "date_finish": "2026-03-05T10:27:29+01:00",
        "operating_reset_at": 1772696249,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of search history items.

The structure of the item object is described in detail [below](#last-item-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Search History Item {#last-item-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`string`](../../data-types.md) 
[`integer`](../../data-types.md) | Identifier of the chat or user identifier for a personal dialog ||
|| **type**
[`string`](../../data-types.md) | Record type: `chat` or `user` ||
|| **avatar**
[`object`](../../data-types.md) | Avatar data of the record.

The structure of the object is described in detail [below](#avatar-object) ||
|| **title**
[`string`](../../data-types.md) | Title of the record ||
|| **user**
[`object`](../../data-types.md) | User data for records of type `user`.

The structure of the object is described in detail [below](#user-object) ||
|| **chat**
[`object`](../../data-types.md) | Chat data for records of type `chat`.

The structure of the object is described in detail [below](#chat-object) ||
|#

### Avatar Object {#avatar-object}

#|
|| **Name**
`Type` | **Description** ||
|| **url**
[`string`](../../data-types.md) | Link to the avatar ||
|| **color**
[`string`](../../data-types.md) | Color in HEX format ||
|#

### User Object {#user-object}

{% include [User Object Tables](../_includes/user-object-tables.md) %}

### Chat Object {#chat-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **name**
[`string`](../../data-types.md) | Name of the chat ||
|| **owner**
[`integer`](../../data-types.md) | Identifier of the chat owner ||
|| **extranet**
[`boolean`](../../data-types.md) | Indicates participation of extranet users in the chat ||
|| **avatar**
[`string`](../../data-types.md) 
[`null`](../../data-types.md) | Link to the chat avatar ||
|| **color**
[`string`](../../data-types.md) | Color of the chat in HEX format ||
|| **type**
[`string`](../../data-types.md) | Type of chat ||
|| **entity_type**
[`string`](../../data-types.md) | Type of the object to which the chat is linked ||
|| **entity_id**
[`string`](../../data-types.md) | Identifier of the object to which the chat is linked ||
|| **entity_data_1**
[`string`](../../data-types.md) | Additional data of the chat object — field 1 ||
|| **entity_data_2**
[`string`](../../data-types.md) | Additional data of the chat object — field 2 ||
|| **entity_data_3**
[`string`](../../data-types.md) | Additional data of the chat object — field 3 ||
|| **mute_list**
[`object`](../../data-types.md) | List of users with muted notifications ||
|| **date_create**
[`string`](../../data-types.md) | Creation date of the chat in ISO 8601 format (RFC3339) ||
|| **message_type**
[`string`](../../data-types.md) | Type of message ||
|#

## Error Handling

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-search-chat-list.md)
- [{#T}](./im-search-department-list.md)
- [{#T}](./im-search-user-list.md)
- [{#T}](./im-search-last-add.md)
- [{#T}](./im-search-last-delete.md)