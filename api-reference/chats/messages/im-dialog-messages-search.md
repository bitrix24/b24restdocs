# Find Message in Chat im.dialog.messages.search

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant

The method `im.dialog.messages.search` performs a search for messages in the chat.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID^*^**
[`integer`](../../data-types.md) | Identifier of the chat in which the search is performed ||
|| **SEARCH_MESSAGE**
[`string`](../../data-types.md) | Search string for the message text.

Search for this parameter is performed for strings longer than 2 characters ||
|| **DATE_FROM**
[`datetime`](../../data-types.md) | Start of the search period in ISO 8601 format (RFC3339) ||
|| **DATE_TO**
[`datetime`](../../data-types.md) | End of the search period in ISO 8601 format (RFC3339) ||
|| **DATE**
[`datetime`](../../data-types.md) | Search for messages on a specific date in ISO 8601 format (RFC3339).

If the parameter is provided, the search is performed within 24 hours from the specified date ||
|| **ORDER**
[`object`](../../data-types.md) | Sorting parameters.

Supported field:
- `ID` — sort by message identifier, values `ASC` or `DESC`

Default: `{"ID": "DESC"}` ||
|| **LIMIT**
[`integer`](../../data-types.md) | Number of messages returned.

Default value: `50`.
Maximum value: `200` ||
|| **LAST_ID**
[`integer`](../../data-types.md) | Identifier of the last message from the previous selection for pagination ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":3,"SEARCH_MESSAGE":"test","ORDER":{"ID":"DESC"},"LIMIT":20}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.dialog.messages.search
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":3,"SEARCH_MESSAGE":"test","ORDER":{"ID":"DESC"},"LIMIT":20,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.dialog.messages.search
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.dialog.messages.search',
            {
                CHAT_ID: 3,
                SEARCH_MESSAGE: 'test',
                ORDER: { ID: 'DESC' },
                LIMIT: 20
            }
        );

        const result = response.getData().result;
        console.log('Search result:', result);
    }
    catch (error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.dialog.messages.search',
                [
                    'CHAT_ID' => 3,
                    'SEARCH_MESSAGE' => 'test',
                    'ORDER' => ['ID' => 'DESC'],
                    'LIMIT' => 20,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.dialog.messages.search',
        {
            CHAT_ID: 3,
            SEARCH_MESSAGE: 'test',
            ORDER: { ID: 'DESC' },
            LIMIT: 20
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.dialog.messages.search',
        [
            'CHAT_ID' => 3,
            'SEARCH_MESSAGE' => 'test',
            'ORDER' => ['ID' => 'DESC'],
            'LIMIT' => 20,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "users": [
            {
                "id": 1,
                "active": true,
                "name": "Alex",
                "firstName": "Alex",
                "lastName": "",
                "workPosition": "",
                "color": "#df532d",
                "avatar": "https://cdn-com.bitrix24.com/path/avatar.jpg",
                "avatarHr": "https://cdn-com.bitrix24.com/path/avatar.jpg",
                "gender": "F",
                "birthday": "",
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "externalAuthId": "socservices",
                "status": "online",
                "idle": false,
                "lastActivityDate": "2026-02-13T14:27:33+01:00",
                "mobileLastDate": false,
                "desktopLastDate": false,
                "absent": false,
                "departments": [1, 107, 47, 3],
                "phones": {
                    "personal_mobile": "19998887766",
                    "inner_phone": "111"
                },
                "botData": null,
                "type": "user",
                "website": "",
                "email": "user@example.com"
            }
        ],
        "files": [],
        "additionalMessages": [],
        "copilot": null,
        "stickers": [],
        "reactions": [],
        "tariffRestrictions": {
            "isHistoryLimitExceeded": false
        },
        "usersShort": [],
        "messages": [
            {
                "id": 33653,
                "chat_id": 2421,
                "author_id": 1,
                "date": "2026-02-13T14:28:00+01:00",
                "text": "test message",
                "isSystem": false,
                "uuid": "18533186-232b-4423-8438-64501da182f5",
                "forward": null,
                "params": [],
                "viewedByOthers": false,
                "unread": false,
                "viewed": true
            }
        ]
    },
    "time": {
        "start": 1770982150,
        "finish": 1770982150.503861,
        "duration": 0.5038609504699707,
        "processing": 0,
        "date_start": "2026-02-13T14:29:10+01:00",
        "date_finish": "2026-02-13T14:29:10+01:00",
        "operating_reset_at": 1770982750,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.messages**
[`array`](../../data-types.md) | Array of found messages [(detailed description)](#message) ||
|| **result.users**
[`array`](../../data-types.md) | Users associated with the found messages [(detailed description)](#user) ||
|| **result.files**
[`array`](../../data-types.md) | Files from the found messages ||
|| **result.additionalMessages**
[`array`](../../data-types.md) | Additional messages related to the found ones, such as forwarded or quoted [(detailed description)](#message) ||
|| **result.copilot**
[`object`](../../data-types.md) | BitrixGPT data, if present in the response. Can be `null` ||
|| **result.stickers**
[`array`](../../data-types.md) | Stickers associated with the found messages ||
|| **result.reactions**
[`array`](../../data-types.md) | Reactions to the found messages [(detailed description)](#reactions) ||
|| **result.tariffRestrictions**
[`object`](../../data-types.md) | Information about tariff restrictions on history.

Contains the flag `isHistoryLimitExceeded` — an indication of exceeding the message history limit by the plan ||
|| **result.usersShort**
[`array`](../../data-types.md) | Brief information about users who reacted.

Used as a supplementary reference to `result.reactions.reactionUsers`, contains `id`, `name`, `avatar` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Message {#message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the message ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **author_id**
[`integer`](../../data-types.md) | Identifier of the message author ||
|| **date**
[`datetime`](../../data-types.md) | Date and time of message creation ||
|| **text**
[`string`](../../data-types.md) | Text of the message ||
|| **isSystem**
[`boolean`](../../data-types.md) | Indicator of a system message ||
|| **uuid**
[`string`](../../data-types.md) | External UUID of the message. Can be `null` ||
|| **forward**
[`object`](../../data-types.md) | Information about forwarding. Can be `null` ||
|| **params**
[`array`](../../data-types.md) | Message parameters ||
|| **viewedByOthers**
[`boolean`](../../data-types.md) | Indicator that the message has been viewed by other participants ||
|| **unread**
[`boolean`](../../data-types.md) | Indicator of an unread message for the current user ||
|| **viewed**
[`boolean`](../../data-types.md) | Indicator that the message has been viewed by the current user ||
|#

#### User {#user}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the user ||
|| **active**
[`boolean`](../../data-types.md) | User is active ||
|| **name**
[`string`](../../data-types.md) | Full name ||
|| **firstName**
[`string`](../../data-types.md) | First name ||
|| **lastName**
[`string`](../../data-types.md) | Last name ||
|| **workPosition**
[`string`](../../data-types.md) | Position ||
|| **color**
[`string`](../../data-types.md) | Profile color in hex format ||
|| **avatar**
[`string`](../../data-types.md) | Avatar URL ||
|| **avatarHr**
[`string`](../../data-types.md) | High-resolution avatar URL ||
|| **gender**
[`string`](../../data-types.md) | Gender ||
|| **birthday**
[`string`](../../data-types.md) | Birthday ||
|| **extranet**
[`boolean`](../../data-types.md) | Indicator of an extranet user ||
|| **network**
[`boolean`](../../data-types.md) | Indicator of a Bitrix24 Network user ||
|| **bot**
[`boolean`](../../data-types.md) | Indicator of a bot ||
|| **connector**
[`boolean`](../../data-types.md) | Indicator of a connector user ||
|| **externalAuthId**
[`string`](../../data-types.md) | External authorization code ||
|| **status**
[`string`](../../data-types.md) | User status ||
|| **idle**
[`boolean`](../../data-types.md) | Indicator of inactivity ||
|| **lastActivityDate**
[`datetime`](../../data-types.md) | Date and time of last activity ||
|| **mobileLastDate**
[`datetime`](../../data-types.md) | Last activity in the mobile app. Can be `false` ||
|| **desktopLastDate**
[`datetime`](../../data-types.md) | Last activity in the desktop app. Can be `false` ||
|| **absent**
[`boolean`](../../data-types.md) | Indicator of absence ||
|| **departments**
[`array`](../../data-types.md) | Array of department identifiers ||
|| **phones**
[`object`](../../data-types.md) | User's phones ||
|| **botData**
[`object`](../../data-types.md) | Additional bot data. For a regular user `null` ||
|| **type**
[`string`](../../data-types.md) | User type ||
|| **website**
[`string`](../../data-types.md) | User's website ||
|| **email**
[`string`](../../data-types.md) | User's e-mail ||
|#

#### Reactions {#reactions}

#|
|| **Name**
`type` | **Description** ||
|| **messageId**
[`integer`](../../data-types.md) | Identifier of the message ||
|| **reactionCounters**
[`object`](../../data-types.md) | Count of reactions by each type ||
|| **reactionUsers**
[`object`](../../data-types.md) | Users by types of reactions ||
|| **ownReactions**
[`array`](../../data-types.md) | Reactions of the current user ||
|#


## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "CHAT_ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | CHAT_ID can't be empty | Required parameter `CHAT_ID` is not provided ||
|| `ACCESS_ERROR` | You do not have access to this chat | No access to the specified chat ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-dialog-messages-get.md)
- [{#T}](./im-dialog-read.md)
- [{#T}](./im-dialog-unread.md)
- [{#T}](./im-dialog-writing.md)
- [{#T}](./im-message-add.md)
- [{#T}](./im-message-update.md)
- [{#T}](./im-message-delete.md)