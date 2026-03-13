# Get User Data with im.user.list.get

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.user.list.get` returns data about users based on a list of identifiers.

If the current user is an extranet user, the method will return data only for users from their extranet groups. User identifiers outside these groups will be skipped without an error.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **ID***
[`array`](../../data-types.md) 
[`string`](../../data-types.md) | An array of user identifiers or a JSON string containing an array.

User identifiers can be obtained using the methods [user.get](../../user/user-get.md), [user.search](../../user/user-search.md), or [im.chat.user.list](../chat-users/im-chat-user-list.md) ||
|| **AVATAR_HR**
[`string`](../../data-types.md) | A parameter to request the `avatar_hr` field with the high-resolution avatar URL. Acceptable values: `Y` or `N`, default is `N`.

Currently, the `avatar_hr` field is always returned, regardless of the parameter value ||
|| **RESULT_TYPE**
[`string`](../../data-types.md) | Format of the `result`. The value `array` will return a standard array of user objects, while any other value will return an object with user identifier keys ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":[4,5],"AVATAR_HR":"Y","RESULT_TYPE":"array"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.user.list.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":[4,5],"RESULT_TYPE":"array","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.user.list.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.user.list.get', {
        ID: [4, 5],
        AVATAR_HR: 'Y',
        RESULT_TYPE: 'array',
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
            'im.user.list.get',
            [
                'ID' => [4, 5],
                'AVATAR_HR' => 'Y',
                'RESULT_TYPE' => 'array',
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
        'im.user.list.get',
        {
            ID: [4, 5],
            AVATAR_HR: 'Y',
            RESULT_TYPE: 'array',
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
        'im.user.list.get',
        [
            'ID' => [4, 5],
            'AVATAR_HR' => 'Y',
            'RESULT_TYPE' => 'array',
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
            "id": 4,
            "active": true,
            "name": "Emily Smith",
            "first_name": "Emily",
            "last_name": "Smith",
            "work_position": "Analyst",
            "color": "#df532d",
            "avatar": "https://example.bitrix24.com/upload/main/avatar4.png",
            "avatar_hr": "https://example.bitrix24.com/upload/main/avatar4_hr.png",
            "gender": "F",
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
            "departments": [7, 5],
            "phones": {
                "work_phone": "+12123456789",
                "personal_mobile": "+12123456789",
                "inner_phone": "22"
            },
            "website": "example.com",
            "email": "emily.smith@example.com",
            "bot_data": null,
            "type": "user"
        },
        {
            "id": 5,
            "active": true,
            "name": "John Johnson",
            "first_name": "John",
            "last_name": "Johnson",
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
                "work_phone": "+12123456788",
                "personal_mobile": "+12123456788",
                "inner_phone": "21"
            },
            "website": "example.com",
            "email": "user@example.com",
            "bot_data": null,
            "type": "user"
        }
    ],
    "time": {
        "start": 1772449081,
        "finish": 1772449081.887056,
        "duration": 0.8870561122894287,
        "processing": 0,
        "date_start": "2026-03-02T13:58:01+01:00",
        "date_finish": "2026-03-02T13:58:01+01:00",
        "operating_reset_at": 1772449681,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) 
[`array`](../../data-types.md) | User data. By default, an object with identifier keys is returned; when `RESULT_TYPE = 'array'`, an array is returned.

The structure of the user object is described in detail [below](#user-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### User Object {#user-object}

{% include [User Object Tables](../_includes/user-object-tables.md) %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "INVALID_FORMAT",
    "error_description": "A wrong format for the ID field is passed"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `INVALID_FORMAT` | A wrong format for the ID field is passed | The `ID` parameter is not provided or is in an incorrect format ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-user-get.md)
- [{#T}](./im-user-status-set.md)
- [{#T}](./im-user-status-get.md)
- [{#T}](./im-user-status-idle-start.md)
- [{#T}](./im-user-status-idle-end.md)