# Get User Data with im.user.get

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute this method: any user

The method `im.user.get` retrieves data about the current user or a user by `ID`.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | User identifier. If not provided, the method will return data for the current user.

You can obtain the user ID using the methods [user.get](../../user/user-get.md), [user.search](../../user/user-search.md), or [im.chat.user.list](../chat-users/im-chat-user-list.md) ||
|| **AVATAR_HR**
[`string`](../../data-types.md) | Parameter to request the `avatar_hr` field with the high-resolution avatar URL. Acceptable values: `Y` or `N`, default is `N`.

Currently, the `avatar_hr` field is always returned, regardless of the parameter value ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":5,"AVATAR_HR":"Y"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.user.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":5,"AVATAR_HR":"Y","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.user.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.user.get', {
        ID: 5,
        AVATAR_HR: 'Y',
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
            'im.user.get',
            [
                'ID' => 5,
                'AVATAR_HR' => 'Y',
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
        'im.user.get',
        {
            ID: 5,
            AVATAR_HR: 'Y',
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
        'im.user.get',
        [
            'ID' => 5,
            'AVATAR_HR' => 'Y',
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
    "result": {
        "id": 5,
        "active": true,
        "name": "John Smith",
        "first_name": "John",
        "last_name": "Smith",
        "work_position": "Manager",
        "color": "#048bd0",
        "avatar": "https://example.bitrix24.com/upload/main/avatar.png",
        "avatar_hr": "https://example.bitrix24.com/upload/main/avatar_hr.png",
        "gender": "M",
        "birthday": "",
        "extranet": false,
        "network": false,
        "bot": false,
        "connector": false,
        "external_auth_id": "default",
        "status": "online",
        "idle": false,
        "last_activity_date": "2026-03-02T09:30:00+01:00",
        "mobile_last_date": false,
        "desktop_last_date": false,
        "absent": false,
        "departments": [10],
        "phones": {
            "work_phone": "+11234567890",
            "personal_mobile": "+11234567890",
            "inner_phone": "21"
        },
        "website": "example.com",
        "email": "user@example.com",
        "bot_data": null,
        "type": "user"
    },
    "time": {
        "start": 1760000000.0,
        "finish": 1760000000.2,
        "duration": 0.2,
        "processing": 0.08,
        "date_start": "2026-03-02T09:30:00+01:00",
        "date_finish": "2026-03-02T09:30:00+01:00",
        "operating_reset_at": 1760030000,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing user data.

The structure of the object is described in detail [below](#result-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Result Object {#result-object}

{% include [User Object Tables](../_includes/user-object-tables.md) %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ID_EMPTY",
    "error_description": "User ID can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ID_EMPTY` | User ID can't be empty | Provided `ID <= 0` ||
|| `USER_NOT_EXISTS` | User does not exist | User with the specified `ID` not found ||
|| `ACCESS_DENIED` | You can request only users who are part of your extranet group | The current extranet user requests a user not from their extranet group ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-user-list-get.md)
- [{#T}](./im-user-status-set.md)
- [{#T}](./im-user-status-get.md)
- [{#T}](./im-user-status-idle-start.md)
- [{#T}](./im-user-status-idle-end.md)