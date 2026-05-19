# Get Information About the Department im.department.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any intranet user, except bots

The method `im.department.get` retrieves data about departments by their `ID` identifiers.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`array`](../../data-types.md) | An array of department identifiers. You can pass a string with a JSON array of identifiers.

You can obtain the department ID using the [get department list method](../../departments/department-get.md) or the [search departments by name method](../search/im-search-department-list.md) ||
|| **USER_DATA**
[`string`](../../data-types.md) | Return data about the department head. 

Possible values:
- `Y` — yes
- `N` — no 
||
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
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.department.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":[3,7],"USER_DATA":"Y","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.department.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.department.get',
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
                'im.department.get',
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
        'im.department.get',
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
        'im.department.get',
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
        "result": [
            {
                "id": 7,
                "name": "Development Department",
                "full_name": "Development Department / mysite.com",
                "manager_user_id": 103
            },
            {
                "id": 3,
                "name": "Finance Department",
                "full_name": "Finance Department / mysite.com",
                "manager_user_id": 13
            }
        ],
        "time": {
            "start": 1772527966,
            "finish": 1772527966.081649,
            "duration": 0.0816490650177002,
            "processing": 0,
            "date_start": "2026-03-03T11:52:46+01:00",
            "date_finish": "2026-03-03T11:52:46+01:00",
            "operating_reset_at": 1772528566,
            "operating": 0
        }
    }
    ```

- When `USER_DATA = 'Y'`:

    ```json
    {
        "result": [
            {
                "id": 7,
                "name": "Development Department",
                "full_name": "Development Department / mysite.com",
                "manager_user_id": 103,
                "manager_user_data": {
                    "id": 103,
                    "active": true,
                    "name": "Anna Peterson",
                    "first_name": "Anna",
                    "last_name": "Peterson",
                    "work_position": "Head of Development Department",
                    "color": "#4ba984",
                    "avatar": "https://mysite.com/upload/avatars/anna-peterson.jpg",
                    "avatar_hr": "https://mysite.com/upload/avatars/anna-peterson.jpg",
                    "gender": "F",
                    "birthday": "",
                    "extranet": false,
                    "network": false,
                    "bot": false,
                    "connector": false,
                    "external_auth_id": "socservices",
                    "status": "online",
                    "idle": false,
                    "last_activity_date": "2025-11-06T16:59:28+01:00",
                    "mobile_last_date": false,
                    "desktop_last_date": false,
                    "absent": false,
                    "departments": [
                        1,
                        7
                    ],
                    "phones": false,
                    "bot_data": null,
                    "type": "user",
                    "website": "",
                    "email": "anna.peterson@mysite.com"
                }
            },
            {
                "id": 3,
                "name": "Finance Department",
                "full_name": "Finance Department / mysite.com",
                "manager_user_id": 13,
                "manager_user_data": {
                    "id": 13,
                    "active": true,
                    "name": "John Smith",
                    "first_name": "John",
                    "last_name": "Smith",
                    "work_position": "Chief Accountant",
                    "color": "#728f7a",
                    "avatar": "https://mysite.com/upload/avatars/john-smith.jpg",
                    "avatar_hr": "https://mysite.com/upload/avatars/john-smith.jpg",
                    "gender": "M",
                    "birthday": "10-09",
                    "extranet": false,
                    "network": false,
                    "bot": false,
                    "connector": false,
                    "external_auth_id": "socservices",
                    "status": "online",
                    "idle": false,
                    "last_activity_date": "2024-02-19T00:40:41+01:00",
                    "mobile_last_date": false,
                    "desktop_last_date": false,
                    "absent": false,
                    "departments": [
                        9,
                        3
                    ],
                    "phones": {
                        "personal_mobile": "12134567890",
                        "inner_phone": "55"
                    },
                    "bot_data": null,
                    "type": "user",
                    "website": "",
                    "email": "john.smith@mysite.com"
                }
            }
        ],
        "time": {
            "start": 1772461967,
            "finish": 1772461967.997741,
            "duration": 0.9977409839630127,
            "processing": 0,
            "date_start": "2026-03-02T17:32:47+01:00",
            "date_finish": "2026-03-02T17:32:47+01:00",
            "operating_reset_at": 1772462567,
            "operating": 0
        }
    }
    ```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of departments [(detailed description)](#department).

When `USER_DATA = 'N'`, it contains a brief description of the departments; when `USER_DATA = 'Y'`, it additionally contains an object with data about the department head ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Department Object {#department}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Department identifier ||
|| **name**
[`string`](../../data-types.md) | Department name ||
|| **full_name**
[`string`](../../data-types.md) | Full name of the department in the company structure ||
|| **manager_user_id**
[`integer`](../../data-types.md) | Identifier of the department head ||
|| **manager_user_data**
[`object`](../../data-types.md) | Data about the department head [(detailed description)](#manager_user_data). Returned only when `USER_DATA = 'Y'` ||
|#

#### Manager User Data Object {#manager_user_data}

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
[`string`](../../data-types.md) | User's position ||
|| **color**
[`string`](../../data-types.md) | User's color in hex format ||
|| **avatar**
[`string`](../../data-types.md) | Link to the avatar ||
|| **avatar_hr**
[`string`](../../data-types.md) | Link to the high-resolution avatar ||
|| **gender**
[`string`](../../data-types.md) | User's gender ||
|| **birthday**
[`string`](../../data-types.md) | Birthday in `DD-MM` format or an empty string ||
|| **extranet**
[`boolean`](../../data-types.md) | Extranet user status ||
|| **network**
[`boolean`](../../data-types.md) | Bitrix24 Network user status ||
|| **bot**
[`boolean`](../../data-types.md) | Bot status ||
|| **connector**
[`boolean`](../../data-types.md) | Open Channels user status ||
|| **external_auth_id**
[`string`](../../data-types.md) | External authorization code ||
|| **status**
[`string`](../../data-types.md) | User status ||
|| **idle**
[`datetime`](../../data-types.md) | User's idle date or `false` ||
|| **last_activity_date**
[`datetime`](../../data-types.md) | User's last activity date ||
|| **mobile_last_date**
[`datetime`](../../data-types.md) | Last activity date in the mobile application or `false` ||
|| **desktop_last_date**
[`datetime`](../../data-types.md) | Last activity date in the desktop application or `false` ||
|| **absent**
[`datetime`](../../data-types.md) | Date of the user's absence end or `false` ||
|| **departments**
[`array`](../../data-types.md) | Array of department identifiers ||
|| **phones**
[`object`](../../data-types.md) | User's phones or `false` [(detailed description)](#phones) ||
|| **bot_data**
[`object`](../../data-types.md) | Bot data or `null` ||
|| **type**
[`string`](../../data-types.md) | User type ||
|| **website**
[`string`](../../data-types.md) | User's website ||
|| **email**
[`string`](../../data-types.md) | User's email ||
|#

#### Object phones {#phones}

#|
|| **Name**
`type` | **Description** ||
|| **personal_mobile**
[`string`](../../data-types.md) | Mobile phone ||
|| **inner_phone**
[`string`](../../data-types.md) | Internal phone ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "INVALID_FORMAT",
    "error_description": "A wrong format for the ID field is passed"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `INVALID_FORMAT` | A wrong format for the ID field is passed | The required parameter `ID` is not provided, is incorrectly passed, or is empty ||
|| `403` | `ACCESS_ERROR` | Only intranet users have access to this method | The method is not available for extranet users and bots ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-department-get.md)
- [{#T}](./im-department-managers-get.md)
- [{#T}](./im-department-employees-get.md)
- [{#T}](./im-department-colleagues-list.md)