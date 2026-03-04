# Get the List of Colleagues for the Current User im.department.colleagues.list

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any intranet user, except bots

The method `im.department.colleagues.list` retrieves the list of colleagues for the current user. For a manager, the method will return a list of subordinates and all supervisors.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **USER_DATA**
[`string`](../../data-types.md) | Return detailed user data. 

Possible values:
- `Y` — yes
- `N` — no ||
|| **OFFSET**
[`integer`](../../data-types.md) | Offset for user selection ||
|| **LIMIT**
[`integer`](../../data-types.md) | Number of items in the selection. Default is `10`. Maximum value is `50` ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_DATA":"Y","OFFSET":0,"LIMIT":5}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.department.colleagues.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_DATA":"Y","OFFSET":0,"LIMIT":5,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.department.colleagues.list
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.department.colleagues.list',
            {
                USER_DATA: 'Y',
                OFFSET: 0,
                LIMIT: 5,
            }
        );

        console.log(response.getData().result);
        console.log(response.getData().total);
        console.log(response.getData().next);
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
                'im.department.colleagues.list',
                [
                    'USER_DATA' => 'Y',
                    'OFFSET' => 0,
                    'LIMIT' => 5,
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
        'im.department.colleagues.list',
        {
            USER_DATA: 'Y',
            OFFSET: 0,
            LIMIT: 5,
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
                console.log(result.total());
                console.log(result.answer.next);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.department.colleagues.list',
        [
            'USER_DATA' => 'Y',
            'OFFSET' => 0,
            'LIMIT' => 5,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

- When `USER_DATA = 'N'`:

    ```json
    {
        "result": [9,547,408,103,290],
        "next": 5,
        "total": 7,
        "time": {
            "start": 1772520802,
            "finish": 1772520802.36194,
            "duration": 0.3619399070739746,
            "processing": 0,
            "date_start": "2026-03-03T09:53:22+01:00",
            "date_finish": "2026-03-03T09:53:22+01:00",
            "operating_reset_at": 1772521402,
            "operating": 0
        }
    }
    ```

- When `USER_DATA = 'Y'`:

    ```json
    {
        "result": [
            {
                "id": 9,
                "active": true,
                "name": "Anna Smith",
                "first_name": "Anna",
                "last_name": "Smith",
                "work_position": "Project Manager",
                "color": "#58cc47",
                "avatar": "https://mysite.com/upload/avatars/anna-smith.jpg",
                "avatar_hr": "https://mysite.com/upload/avatars/anna-smith.jpg",
                "gender": "F",
                "birthday": "",
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "external_auth_id": "socservices",
                "status": "online",
                "idle": false,
                "last_activity_date": "2023-03-10T17:16:44+01:00",
                "mobile_last_date": false,
                "desktop_last_date": false,
                "absent": false,
                "departments": [
                    1,
                    667
                ],
                "phones": false,
                "bot_data": null,
                "type": "user",
                "website": "",
                "email": "anna.smith@mysite.com"
            },
            ... // description for each user
        ],
        "next": 5,
        "total": 7,
        "time": {
            "start": 1772521273,
            "finish": 1772521273.83899,
            "duration": 0.8389899730682373,
            "processing": 0,
            "date_start": "2026-03-03T10:01:13+01:00",
            "date_finish": "2026-03-03T10:01:13+01:00",
            "operating_reset_at": 1772521873,
            "operating": 0
        }
    }
    ```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of users. 
- When `USER_DATA = 'N'`, contains user IDs
- When `USER_DATA = 'Y'`, contains user objects [(detailed description)](#user-object) ||
|| **total**
[`integer`](../../data-types.md) | Total number of users in the selection ||
|| **next**
[`integer`](../../data-types.md) | Offset for retrieving the next page. This field is returned if there is a next page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### User Object {#user-object}

{% include [User Object Tables](./_includes/user-object-tables.md) %}

## Error Handling

HTTP Status: **403**

```json
{
    "error": "ACCESS_ERROR",
    "error_description": "Only intranet users have access to this method."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_ERROR` | Only intranet users have access to this method | The method is not available for extranet users and bots ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-department-get.md)
- [{#T}](./im-department-managers-get.md)
- [{#T}](./im-department-employees-get.md)
- [{#T}](./im-department-colleagues-list.md)