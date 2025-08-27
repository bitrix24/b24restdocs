# Get User Data im.user.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

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

The method `im.user.get` retrieves user data.

#| 
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ID** 
[`unknown`](../../data-types.md) | `5` | User identifier | 18 ||
|| **AVATAR_HR** 
[`unknown`](../../data-types.md) | `N` | Generate avatar in high resolution | 18 ||
|#

If the `ID` key is not provided, data for the current user will be selected.

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.user.get',
            {
                ID: 5
            }
        );
        
        const result = response.getData().result;
        console.log(result);
    }
    catch(error)
    {
        console.error(error.ex);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.user.get',
                [
                    'ID' => 5
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error()->ex;
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting user information: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'im.user.get',
        {
            ID: 5
        },
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

- PHP CRest

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.user.get',
        Array(
            'ID' => 5,
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

- cURL

    // example for cURL

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Example Response

```json
{
    "result": {
        "id": 5,
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
        "last_activity_date": "2018-01-29T17:35:31+02:00",
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
```

### Key Descriptions

- `id` – user identifier
- `name` – user's full name
- `first_name` – user's first name
- `last_name` – user's last name
- `work_position` – position
- `color` – user color in hex format
- `avatar` – link to avatar (if empty, avatar is not set)
- `avatar_hr` – link to high-resolution avatar (available only when requesting with parameter `AVATAR_HR = 'Y'`)
- `gender` – user's gender
- `birthday` – user's birthday in DD-MM format, if empty – not set
- `extranet` – indicator of external extranet user (`true/false`)
- `network` – indicator of Bitrix24.Network user (`true/false`)
- `bot` – indicator of bot (`true/false`)
- `connector` – indicator of open channel user (`true/false`)
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
    "error": "ID_EMPTY",
    "error_description": "User ID can't be empty"
}
```

### Key Descriptions

- `error` – error code
- `error_description` – brief description of the error

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| **ID_EMPTY** | User identifier not provided ||
|| **USER_NOT_EXISTS** | User with the specified identifier not found ||
|| **ACCESS_DENIED** | Current user does not have access permission to the data ||
|#