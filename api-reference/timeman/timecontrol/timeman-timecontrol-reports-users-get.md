# Get the list of users for the timeman.timecontrol.reports.users.get

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: department head or administrator

The method `timeman.timecontrol.reports.users.get` retrieves the list of users in the department.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **DEPARTMENT_ID**
[`integer`](../../data-types.md) | Department identifier. This parameter should only be specified by the department head or administrator.

You can obtain the department identifier using the [get department list](../../departments/department-get.md) method ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DEPARTMENT_ID":15}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.timecontrol.reports.users.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DEPARTMENT_ID":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.timecontrol.reports.users.get
    ```

- JS

    ```js
    BX24.callMethod(
        'timeman.timecontrol.reports.users.get',
        {
            'DEPARTMENT_ID': 15
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.timecontrol.reports.users.get',
        [
            'DEPARTMENT_ID' => 15
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "id": 3,
            "name": "Maria Ivshina",
            "first_name": "Maria",
            "last_name": "Ivshina",
            "work_position": "IT Specialist",
            "avatar": "http://test.bitrix24.com/upload/resize_cache/45749/7acf4ca766af5d8/main/c89/c89c6b73470635c/4R5A1256.png",
            "personal_gender": "F",
            "last_activity_date": "2025-05-29T16:41:00+02:00"
        }
    ],
    "time": {
        "start": 1748526089.625516,
        "finish": 1748526089.656787,
        "duration": 0.03127098083496094,
        "processing": 0.008746147155761719,
        "date_start": "2025-05-29T16:41:29+02:00",
        "date_finish": "2025-05-29T16:41:29+02:00",
        "operating_reset_at": 1748526689,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of users ||
|| **id**
[`integer`](../../data-types.md) | User identifier ||
|| **name**
[`string`](../../data-types.md) | Full name of the user ||
|| **first_name**
[`string`](../../data-types.md) | First name ||
|| **last_name**
[`string`](../../data-types.md) | Last name ||
|| **work_position**
[`string`](../../data-types.md) | Job title ||
|| **avatar**
[`string`](../../data-types.md) | User's avatar URL.

If the value is empty, the user does not have an avatar ||
|| **personal_gender**
[`string`](../../data-types.md) | Gender ||
|| **last_activity_date**
[`string`](../../data-types.md) | Date of the user's last activity in ATOM format ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You don't have access to this method"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_ERROR` | You don't have access to this method | You do not have access to this method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-timecontrol-report-add.md)
- [{#T}](./timeman-timecontrol-reports-get.md)