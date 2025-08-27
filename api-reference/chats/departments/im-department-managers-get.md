# Get the list of department managers im.department.managers.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

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

The method `im.department.managers.get` retrieves a list of department managers.

#| 
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ID^*^**
[`unknown`](../../data-types.md) | `[105]` | Department identifiers | 19 ||
|| **USER_DATA**
[`unknown`](../../data-types.md) | `N` | Load user data | 19 ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

- If the parameter `USER_DATA = Y` is passed, the response will return an array of objects with user information instead of an array of identifiers.

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.department.managers.get',
            {
                ID: [7],
                USER_DATA: 'Y'
            }
        );
        
        const result = response.getData().result;
        console.log('users', result);
    }
    catch( error )
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
                'im.department.managers.get',
                [
                    'ID'       => [7],
                    'USER_DATA' => 'Y',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error()->ex;
        } else {
            echo 'users: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting department managers: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.department.managers.get',
        {
            ID: [7],
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
            }
        }
    );
    ```

- PHP CRest

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.department.managers.get',
        Array(
            'ID' => [7],
            'USER_DATA' => 'Y'
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

- cURL

    // example for cURL

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Successful response

With the option `USER_DATA = N`:

```json
{
    "result": {
        105: [1]
    },
}    
```

With the option `USER_DATA = Y`:

```json
{    
    "result": {
        105: {
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

### Key descriptions

- `id` – user identifier
- `name` – user's full name
- `first_name` – user's first name
- `last_name` – user's last name
- `work_position` – position
- `color` – user color in hex format
- `avatar` – link to avatar (if empty, avatar is not set)
- `gender` – user's gender
- `birthday` – user's birthday in DD-MM format, if empty – not set
- `extranet` – indicator of external extranet user (`true/false`)
- `network` – indicator of Bitrix24.Network user (`true/false`)
- `bot` – indicator of bot (`true/false`)
- `connector` – indicator of open channel user (`true/false`)
- `external_auth_id` – external authorization code
- `status` – user status. Always displayed as online, even if the user has set the status to "Do Not Disturb". The "Do Not Disturb" status only affects notification receipt and is not visible to other users
- `idle` – date when the user stepped away from the computer, in ATOM format (if not set, `false`)
- `last_activity_date` – date of the user's last action in ATOM format
- `mobile_last_date` – date of the last action in the mobile application in ATOM format (if not set, `false`)
- `desktop_last_date` – date of the last action in the desktop application in ATOM format (if not set, `false`)
- `absent` – date until which the user is on vacation, in ATOM format (if not set, `false`)
- `phones` – array of phone numbers: `work_phone` – work phone, `personal_mobile` – mobile phone, `personal_phone` – home phone

## Error response

```json
{
    "error": "ID_EMPTY",
    "error_description": "Department ID can't be empty"
}
```

### Key descriptions

- `error` – error code
- `error_description` – brief description of the error

### Possible error codes

#| 
|| **Code** | **Description** ||
|| **ID_EMPTY** | List of identifiers not provided ||
|#