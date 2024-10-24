# Get a list of users in your department im.department.colleagues.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.department.colleagues.list` retrieves a list of users in your department.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **USER_DATA**
[`unknown`](../../data-types.md) | `N` | Load user data | 19 ||
|| **OFFSET**
[`unknown`](../../data-types.md) | `0` | Offset for user selection | 19 ||
|| **LIMIT**
[`unknown`](../../data-types.md) | `10` | Limit for user selection | 19 ||
|#

- If the parameter `USER_DATA = Y` is passed, the response will return an array of objects with user information instead of an array of identifiers.
- The method supports standard pagination of the Bitrix24 Rest API, but in addition, it allows navigation using the `OFFSET` and `LIMIT` parameters.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.department.colleagues.list',
        {
            USER_DATA: 'Y'
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
        'im.department.colleagues.list',
        Array(
            'USER_DATA' => 'Y'
        ),
        $_REQUEST[
            "auth"
        ]
    );    
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Successful response

With the option `USER_DATA = N`:

```json
{
    "result": [1],
    "total": 1
}    
```

With the option `USER_DATA = Y`:

```json
{    
    "result": [
        {
            "id": 1,
            "name": "Eugene Shelenkov",
            "first_name": "Eugene",
            "last_name": "Shelenkov",
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
            "last_activity_date": "2018-01-29T17:35:31+01:00",
            "desktop_last_date": false,
            "mobile_last_date": false,
            "departments": [
             50
            ],
            "absent": false,
            "phones": {
             "work_phone": "",
             "personal_mobile": "",
             "personal_phone": ""
            }
        }
    ],
    "total": 1
}    
```

### Description of keys

- `id` – user identifier
- `name` – user's full name
- `first_name` – user's first name
- `last_name` – user's last name
- `work_position` – job title
- `color` – user color in hex format
- `avatar` – link to avatar (if empty, avatar is not set)
- `gender` – user's gender
- `birthday` – user's birthday in DD-MM format, if empty – not set
- `extranet` – indicator of external extranet user (`true/false`)
- `network` – indicator of Bitrix24.Network user (`true/false`)
- `bot` – indicator of bot (`true/false`)
- `connector` – indicator of open lines user (`true/false`)
- `external_auth_id` – external authorization code
- `status` – user status. Always displayed as online, even if the user has set the status to "Do Not Disturb". The "Do Not Disturb" status only affects notification receipt and is not visible to other users
- `idle` – date when the user stepped away from the computer, in ATOM format (if not set, `false`)
- `last_activity_date` – date of the user's last action in ATOM format
- `desktop_last_date` – date of the last action in the desktop application in ATOM format (if not set, `false`)
- `mobile_last_date` – date of the last action in the mobile application in ATOM format (if not set, `false`)
- `absent` – date until which the user is on vacation, in ATOM format (if not set, `false`)
- `phones` – array of phone numbers: `work_phone` – work phone, `personal_mobile` – mobile phone, `personal_phone` – home phone