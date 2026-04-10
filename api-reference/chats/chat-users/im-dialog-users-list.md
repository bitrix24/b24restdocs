# Get the List of Participants im.dialog.users.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the chat

The method `im.dialog.users.list` returns detailed information about the participants in the dialog with pagination support.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the chat in the format:

- `chatXXX` — chat
- `sgXXX` — group or project chat
- `XXX` — identifier of the personal chat user
  
The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method. The user identifier can be retrieved using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods ||
|| **SKIP_EXTERNAL**
[`string`](../../data-types.md) | Exclude system users:
- `Y` — exclude
- `N` — do not exclude

Default: `N` ||
|| **SKIP_EXTERNAL_EXCEPT_TYPES**
[`string`](../../data-types.md) | A list of types of system users to keep in the selection, separated by commas.

For example: `bot, email` ||
|| **LIMIT**
[`integer`](../../data-types.md) | Number of items per page. Default: `50`. Maximum value: `200` ||
|| **LAST_ID**
[`integer`](../../data-types.md) | Identifier of the last user from the selection `LIMIT`.

Used for sequential loading of the list ||
|| **OFFSET**
[`integer`](../../data-types.md) | Offset for pagination. Default: `0` ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2935","SKIP_EXTERNAL":"Y","LIMIT":20,"OFFSET":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.dialog.users.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2935","SKIP_EXTERNAL":"Y","LIMIT":20,"OFFSET":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.dialog.users.list
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.dialog.users.list',
            {
                DIALOG_ID: 'chat13',
                SKIP_EXTERNAL: 'Y',
                LIMIT: 20,
                start: 0
            }
        );

        console.log(response.getData().result);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.dialog.users.list',
                [
                    'DIALOG_ID' => 'chat2935',
                    'SKIP_EXTERNAL' => 'Y',
                    'LIMIT' => 20,
                    'OFFSET' => 0
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving dialog users list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.dialog.users.list',
        {
            DIALOG_ID: 'chat2935',
            SKIP_EXTERNAL: 'Y',
            LIMIT: 20,
            OFFSET: 0
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
        'im.dialog.users.list',
        [
            'DIALOG_ID' => 'chat2935',
            'SKIP_EXTERNAL' => 'Y',
            'LIMIT' => 20,
            'OFFSET' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
        "id": 1269,
        "active": true,
        "name": "Alexander Smith",
        "first_name": "Alexander",
        "last_name": "Smith",
        "work_position": "Sales Department Head",
        "color": "#58cc47",
        "avatar": "https://cdn-com.bitrix24.com/avatars/alexander-smith.png",
        "avatar_hr": "https://cdn-com.bitrix24.com/avatars/alexander-smith@2x.png",
        "gender": "M",
        "birthday": "",
        "extranet": false,
        "network": false,
        "bot": false,
        "connector": false,
        "external_auth_id": "socservices",
        "status": "online",
        "idle": false,
        "last_activity_date": "2026-03-02T17:45:59+02:00",
        "mobile_last_date": false,
        "desktop_last_date": false,
        "absent": false,
        "departments": [1],
        "phones": {
            "personal_mobile": "+19165554419"
        },
        "bot_data": null,
        "type": "user",
        "website": "",
        "email": "a.smith@example.com"
        },
        {
        "id": 99,
        "active": true,
        "name": "Catherine Peterson",
        "first_name": "Catherine",
        "last_name": "Peterson",
        "work_position": "Business Analyst",
        "color": "#58cc47",
        "avatar": "https://cdn-com.bitrix24.com/avatars/catherine-peterson.png",
        "avatar_hr": "https://cdn-com.bitrix24.com/avatars/catherine-peterson@2x.png",
        "gender": "F",
        "birthday": "",
        "extranet": false,
        "network": false,
        "bot": false,
        "connector": false,
        "external_auth_id": "socservices",
        "status": "online",
        "idle": false,
        "last_activity_date": "2026-03-02T17:44:31+02:00",
        "mobile_last_date": false,
        "desktop_last_date": false,
        "absent": false,
        "departments": [121],
        "phones": false,
        "bot_data": null,
        "type": "user",
        "website": "",
        "email": "c.peterson@mail.com"
        },
        {
        "id": 1271,
        "active": true,
        "name": "Dmitry Harris",
        "first_name": "Dmitry",
        "last_name": "Harris",
        "work_position": "Senior Developer",
        "color": "#df532d",
        "avatar": "https://cdn-com.bitrix24.com/avatars/dmitry-harris.png",
        "avatar_hr": "https://cdn-com.bitrix24.com/avatars/dmitry-harris@2x.png",
        "gender": "M",
        "birthday": "",
        "extranet": false,
        "network": false,
        "bot": false,
        "connector": false,
        "external_auth_id": "socservices",
        "status": "online",
        "idle": false,
        "last_activity_date": "2026-03-02T17:45:02+02:00",
        "mobile_last_date": false,
        "desktop_last_date": false,
        "absent": false,
        "departments": [1],
        "phones": false,
        "bot_data": null,
        "type": "user",
        "website": "",
        "email": "d.harris@mail.com"
        }
    ],
    "total": 3,
    "time": {
        "start": 1772477165,
        "finish": 1772477165.266759,
        "duration": 0.26675891876220703,
        "processing": 0,
        "date_start": "2026-03-02T17:46:05+02:00",
        "date_finish": "2026-03-02T17:46:05+02:00",
        "operating_reset_at": 1772477765,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | An array of objects with [dialog participant data](#result) ||
|| **total**
[`integer`](../../data-types.md) | Total number of participants available to the current user ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page of results.

Returned if there is a next page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | User identifier ||
|| **active**
[`boolean`](../../data-types.md) | User activity status ||
|| **name**
[`string`](../../data-types.md) | User's full name ||
|| **first_name**
[`string`](../../data-types.md) | User's first name ||
|| **last_name**
[`string`](../../data-types.md) | User's last name ||
|| **work_position**
[`string`](../../data-types.md) | User's position. Can be `null` ||
|| **color**
[`string`](../../data-types.md) | User's color in HEX format ||
|| **avatar**
[`string`](../../data-types.md) | Link to the avatar. If empty, the avatar is not set ||
|| **avatar_hr**
[`string`](../../data-types.md) | Link to the high-resolution avatar ||
|| **gender**
[`string`](../../data-types.md) | User's gender ||
|| **birthday**
[`string`](../../data-types.md) | Birthday in `DD-MM` format or an empty string ||
|| **extranet**
[`boolean`](../../data-types.md) | Indicator of an external extranet user ||
|| **network**
[`boolean`](../../data-types.md) | Indicator of a Bitrix24.Network user ||
|| **bot**
[`boolean`](../../data-types.md) | Indicator of a bot ||
|| **connector**
[`boolean`](../../data-types.md) | Indicator of an Open Channels user ||
|| **external_auth_id**
[`string`](../../data-types.md) | External authorization code ||
|| **status**
[`string`](../../data-types.md) | User status ||
|| **idle**
[`datetime`](../../data-types.md) | Date when the user stepped away from the computer. If not set, `false` ||
|| **last_activity_date**
[`datetime`](../../data-types.md) | Date of the user's last activity ||
|| **mobile_last_date**
[`datetime`](../../data-types.md) | Date of the last activity in the mobile application. If not set, `false` ||
|| **desktop_last_date**
[`datetime`](../../data-types.md) | Date of the last activity in the desktop application. If not set, `false` ||
|| **absent**
[`datetime`](../../data-types.md) | Date of the user's absence. If not set, `false` ||
|| **departments**
[`array`](../../data-types.md) | List of user department identifiers ||
|| **phones**
[`object`](../../data-types.md) | Contact [user phones](#result-phones). Can be `false` ||
|| **bot_data**
[`object`](../../data-types.md) | Bot data. For a regular user, it can be `null` ||
|| **type**
[`string`](../../data-types.md) | User type ||
|| **website**
[`string`](../../data-types.md) | User's website ||
|| **email**
[`string`](../../data-types.md) | User's email ||
|#

#### Phones Object {#result-phones}

#|
|| **Name**
`type` | **Description** ||
|| **work_phone**
[`string`](../../data-types.md) | Work phone ||
|| **personal_mobile**
[`string`](../../data-types.md) | Mobile phone ||
|| **personal_phone**
[`string`](../../data-types.md) | Home phone ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `DIALOG_ID_EMPTY` | Dialog ID can't be empty | The `DIALOG_ID` was not provided or is invalid ||
|| `ACCESS_ERROR` | You do not have access to the specified dialog | No access to the specified dialog ||
|| `ACCESS_ERROR` | You don't have access to this chat | No access to the specified chat ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../im-chat-add.md)
- [{#T}](./im-chat-user-add.md)
- [{#T}](../chat-update/im-chat-update-title.md)
- [{#T}](../chat-update/im-chat-update-avatar.md)
- [{#T}](../chat-update/im-chat-update-color.md)
- [{#T}](../im-chat-get.md)
- [{#T}](../im-dialog-get.md)
- [{#T}](./im-chat-user-list.md)
- [{#T}](./im-chat-user-delete.md)
- [{#T}](./im-chat-leave.md)