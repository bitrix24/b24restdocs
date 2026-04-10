# Find Departments im.search.department.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.department.list` performs a search for departments by their full name.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **FIND***  
[`string`](../../data-types.md) | Search phrase for finding the full name of the department (field [full_name](../departments/im-department-get.md#department)) ||
|| **USER_DATA**  
[`string`](../../data-types.md) | Return the manager's data in the field [manager_user_data](#manager_user_data). 

Available values: 
- `Y` — yes
- `N` — no

Default value — `N` ||
|| **OFFSET**  
[`integer`](../../data-types.md) | Offset for the department selection. Default is `0` ||
|| **LIMIT**  
[`integer`](../../data-types.md) | Number of items in the selection. Default is `10`. Maximum value is `50` ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"FIND":"Department","USER_DATA":"Y","OFFSET":0,"LIMIT":10}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.search.department.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"FIND":"Department","USER_DATA":"Y","OFFSET":0,"LIMIT":10,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.search.department.list
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.search.department.list', {
        FIND: 'Department',
        USER_DATA: 'Y',
        OFFSET: 0,
        LIMIT: 10,
      });

      const { result, total, next } = response.getData();
      console.log(result, total, next);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.search.department.list',
            [
                'FIND' => 'Department',
                'USER_DATA' => 'Y',
                'OFFSET' => 0,
                'LIMIT' => 10,
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
        'im.search.department.list',
        {
            FIND: 'Department',
            USER_DATA: 'Y',
            OFFSET: 0,
            LIMIT: 10,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data(), result.total(), result.next());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.search.department.list',
        [
            'FIND' => 'Department',
            'USER_DATA' => 'Y',
            'OFFSET' => 0,
            'LIMIT' => 10,
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
            "id": 9,
            "name": "Marketing and Advertising Department",
            "full_name": "Marketing and Advertising Department / My Company",
            "manager_user_id": 3,
            "manager_user_data": {
                "id": 3,
                "active": true,
                "name": "Catherine Peterson",
                "first_name": "Catherine",
                "last_name": "Peterson",
                "work_position": "",
                "color": "#1eb4aa",
                "avatar": "https://example.bitrix24.com/upload/main/avatar.png",
                "avatar_hr": "https://example.bitrix24.com/upload/main/avatar.png",
                "gender": "F",
                "birthday": "06-04",
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "external_auth_id": "socservices",
                "status": "online",
                "idle": false,
                "last_activity_date": "2026-03-04T22:08:29+01:00",
                "mobile_last_date": false,
                "desktop_last_date": false,
                "absent": false,
                "departments": [1],
                "phones": {
                    "work_phone": "1415551111",
                    "inner_phone": "222"
                },
                "bot_data": null,
                "type": "user",
                "website": "example.com",
                "email": "user@example.com"
            }
        },
        ... // description for each department
    ],
    "total": 2,
    "time": {
        "start": 1772651443,
        "finish": 1772651443.378436,
        "duration": 0.3784360885620117,
        "processing": 0,
        "date_start": "2026-03-04T22:10:43+01:00",
        "date_finish": "2026-03-04T22:10:43+01:00",
        "operating_reset_at": 1772652043,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**  
[`array`](../../data-types.md) | List of found departments.

The structure of the department object is described in detail [below](#department-object) ||
|| **total**  
[`integer`](../../data-types.md) | Total number of found departments ||
|| **next**  
[`integer`](../../data-types.md) | Offset for the next page. This field is returned if there is a next page ||
|| **time**  
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

### Department Object {#department-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**  
[`integer`](../../data-types.md) | Department identifier ||
|| **name**  
[`string`](../../data-types.md) | Short name of the department ||
|| **full_name**  
[`string`](../../data-types.md) | Full name of the department ||
|| **manager_user_id**  
[`integer`](../../data-types.md) | Identifier of the department manager ||
|| **manager_user_data**  
[`object`](../../data-types.md) | Data of the department manager. This object is returned only when `USER_DATA = 'Y'`.

The structure of the manager object is described in detail [below](#manager_user_data) ||
|#

#### Manager User Data Object {#manager_user_data}

{% include [User Object Tables](../_includes/user-object-tables.md) %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "FIND_SHORT",
    "error_description": "Too short a search phrase."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `FIND_SHORT` | Too short a search phrase | The `FIND` parameter is not provided or the phrase is less than three characters ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-search-chat-list.md)
- [{#T}](./im-search-user-list.md)
- [{#T}](./im-search-last-add.md)
- [{#T}](./im-search-last-get.md)
- [{#T}](./im-search-last-delete.md)