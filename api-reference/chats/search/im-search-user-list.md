# Find Users im.search.user.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.user.list` performs a search for users.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **FIND^*^**
[`unknown`](../../data-types.md) | `Eugene` | Search phrase | 19 ||
|| **BUSINESS**
[`unknown`](../../data-types.md) | `N` | Search among business users | 19 ||
|| **AVATAR_HR**
[`unknown`](../../data-types.md) | `N` | Generate avatar in high resolution | 19 ||
|| **OFFSET**
[`unknown`](../../data-types.md) | `0` | Offset for user selection | 19 ||
|| **LIMIT**
[`unknown`](../../data-types.md) | `10` | Limit for user selection | 19 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

- The search is conducted across the following fields: **First Name**, **Last Name**, **Position**, **Department**.
- The method supports standard pagination of the Bitrix24 Rest API, but in addition, it allows navigation using the `OFFSET` and `LIMIT` parameters.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.search.user.list',
        {
            FIND: 'Eugene'
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
        'im.search.user.list',
        Array(
            'FIND' => 'Eugene'
        ),
        $_REQUEST[
            "auth"
        ]
    );    
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}

## Successful Response

```json
{    
    "result": {
        1: {
            "id": 1,
            "name": "Eugene Shelenkov",
            "first_name": "Eugene",
            "last_name": "Shelankov",
            "work_position": "",
            "color": "#df532d",
            "avatar": "http://192.168.2.232/upload/resize_cache/main/1d3/100_100_2/shelenkov.png",
            "gender": "M",
            "birthday": "",
            "extranet": false,
            "network": false,
            "bot": false,
            "connector": false,
            "external_auth_id": "default",
            "status": "online",
            "idle": false,
            "last_activity_date": "2018-01-29T17:35:31+03:00",
            "desktop_last_date": false,
            "mobile_last_date": false,
            "departments": [
             50
            ],
            "absent": false
        }
    },
    "total": 1
}    
```

### Key Descriptions

- `id` – user identifier
- `name` – user's full name
- `first_name` – user's first name
- `last_name` – user's last name
- `work_position` – position
- `color` – user's color in hex format
- `avatar` – link to avatar (if empty, avatar is not set)
- `avatar_hr` – link to high-resolution avatar (available only when requested with `AVATAR_HR = 'Y'`)
- `gender` – user's gender
- `birthday` – user's birthday in DD-MM format, if empty – not set
- `extranet` – indicator of external extranet user (`true/false`)
- `network` – indicator of Bitrix24.Network user (`true/false`)
- `bot` – indicator of bot (`true/false`)
- `connector` – indicator of open lines user (`true/false`)
- `external_auth_id` – external authorization code
- `status` – selected user status
- `idle` – date when the user stepped away from the computer, in ATOM format (if not set, `false`)
- `last_activity_date` – date of the user's last action in ATOM format
- `mobile_last_date` – date of the last action in the mobile app in ATOM format (if not set, `false`)
- `desktop_last_date` – date of the last action in the desktop app in ATOM format (if not set, `false`)
- `absent` – date until which the user is on vacation, in ATOM format (if not set, `false`)

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
|| **FIND_SHORT** | Search phrase is too short; search requires at least three characters. ||
|#