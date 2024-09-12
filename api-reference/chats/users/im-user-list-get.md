# Get User Data im.user.list.get

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

The method `im.user.list.get` retrieves user data.

## Parameters

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ID^*^**
[`unknown`](../../data-types.md) | `[4,5]` | User identifiers | 19 ||
|| **AVATAR_HR**
[`unknown`](../../data-types.md) | `N` | Generate avatar in high resolution | 19 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

If the `ID` key is not provided, data for the current user will be selected.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```javascript
    BX24.callMethod(
        'im.user.list.get',
        {ID: [4,5]},
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    {% include [Explanation of restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.user.list.get',
        Array(
            'ID' => [4,5],
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result": {
        "4": null,
        "5": {
            "id": 5,
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
            "last_activity_date": "2018-01-29T17:35:31+03:00",
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
    }
}
```

### Key Descriptions

- `id` – user identifier
- `name` – user's full name
- `first_name` – user's first name
- `last_name` – user's last name
- `work_position` – job title
- `color` – user's color in hex format
- `avatar` – link to the avatar (if empty, avatar is not set)
- `avatar_hr` – link to the high-resolution avatar (available only when requested with the parameter `AVATAR_HR = 'Y'`)
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
- `departments` – department identifiers
- `desktop_last_date` – date of the last action in the desktop app in ATOM format (if not set, `false`)
- `absent` – date until which the user is on vacation, in ATOM format (if not set, `false`)
- `phones` – array of phone numbers: `work_phone` – work phone, `personal_mobile` – mobile phone, `personal_phone` – home phone

## Error Response

```json
{
    "error": "INVALID_FORMAT",
    "error_description": "A wrong format for the ID field is passed"
}
```

### Key Descriptions

- `error` – error code
- `error_description` – brief description of the error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **INVALID_FORMAT** | Incorrect format for the ID field ||
|#