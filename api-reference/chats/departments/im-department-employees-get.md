# Get a List of Employees from Departments im.department.employees.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any intranet user, except bots

The method `im.department.employees.get` retrieves a list of employees from the specified departments.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`array`](../../data-types.md) | An array of department IDs. You can pass a string with a JSON array of IDs.

You can obtain the department ID using the [get department list method](../../departments/department-get.md) or the [search departments by name method](../search/im-search-department-list.md) ||
|| **USER_DATA**
[`string`](../../data-types.md) | Return detailed user data.  

Possible values:
- `Y` — yes,
- `N` — no ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":[3,7],"USER_DATA":"Y"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.department.employees.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":[3,7],"USER_DATA":"Y","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.department.employees.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.department.employees.get',
            {
                ID: [3, 7],
                USER_DATA: 'Y'
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
                'im.department.employees.get',
                [
                    'ID' => [3, 7],
                    'USER_DATA' => 'Y',
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
        'im.department.employees.get',
        {
            ID: [3, 7],
            USER_DATA: 'Y'
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
        'im.department.employees.get',
        [
            'ID' => [3, 7],
            'USER_DATA' => 'Y',
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
        "result": {
            "7": [3,67,61,103],
            "3": [278,517,13]
        },
        "time": {
            "start": 1772519829,
            "finish": 1772519829.456118,
            "duration": 0.456118106842041,
            "processing": 0,
            "date_start": "2026-03-03T09:37:09+01:00",
            "date_finish": "2026-03-03T09:37:09+01:00",
            "operating_reset_at": 1772520429,
            "operating": 0.18364310264587402
        }
    }
    ```

- When `USER_DATA = 'Y'`:

    ```json
    {
        "result": {
            "7": [
                {
                    "id": 3,
                    "active": true,
                    "name": "John Smith",
                    "first_name": "John",
                    "last_name": "Smith",
                    "work_position": "System Administrator",
                    "color": "#4ba984",
                    "avatar": "https://mysite.com/upload/avatars/john-smith.jpg",
                    "avatar_hr": "https://mysite.com/upload/avatars/john-smith.jpg",
                    "gender": "M",
                    "birthday": "",
                    "extranet": false,
                    "network": false,
                    "bot": false,
                    "connector": false,
                    "external_auth_id": "socservices",
                    "status": "online",
                    "idle": false,
                    "last_activity_date": "2015-02-16T19:41:09+01:00",
                    "mobile_last_date": false,
                    "desktop_last_date": false,
                    "absent": false,
                    "departments": [
                        7
                    ],
                    "phones": {
                        "inner_phone": "42"
                    },
                    "bot_data": null,
                    "type": "user",
                    "website": "",
                    "email": "john.smith@mysite.com"
                },
                {
                    "id": 67,
                    "active": true,
                    "name": "Anna Johnson",
                    "first_name": "Anna",
                    ...
                },

            ],
            "3": [
                {
                    "id": 278,
                    "active": true,
                    "name": "Maria Brown",
                    "first_name": "Maria",
                    "last_name": "Brown",
                    ...
                },
            ...
            ]
        },
        "time": {
            "start": 1772519820,
            "finish": 1772519820.48721,
            "duration": 0.4872100353240967,
            "processing": 0,
            "date_start": "2026-03-03T09:37:00+01:00",
            "date_finish": "2026-03-03T09:37:00+01:00",
            "operating_reset_at": 1772520420,
            "operating": 0.18364310264587402
        }
    }
    ```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root object where the key is the department ID, and the value:
- when `USER_DATA = 'N'` contains an array of employee IDs,
- when `USER_DATA = 'Y'` contains an array of objects with user descriptions [(detailed description)](#user-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### User Object {#user-object}

{% include [User Object Tables](./_includes/user-object-tables.md) %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ID_EMPTY",
    "error_description": "Department ID can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ID_EMPTY` | Department ID can't be empty | The required parameter `ID` is missing, incorrectly provided, or empty ||
|| `403` | `ACCESS_ERROR` | Only intranet users have access to this method | The method is not available for extranet users and bots ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-department-get.md)
- [{#T}](./im-department-managers-get.md)
- [{#T}](./im-department-employees-get.md)
- [{#T}](./im-department-colleagues-list.md)