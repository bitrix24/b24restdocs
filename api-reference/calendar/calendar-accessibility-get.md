# Get User Availability from calendar.accessibility.get

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves the availability of users from the list.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **users***
[`array`](../data-types.md) | Array of user IDs ||
|| **from***
[`date`](../data-types.md) | Start date of the period for determining availability in the format `YYYY-MM-DD`.

For example, `2024-06-20` ||
|| **to***
[`date`](../data-types.md) | End date of the period for determining availability in the format `YYYY-MM-DD`.

For example, `2024-12-20`  ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"from":"2024-06-20","to":"2024-12-20","users":[1,2,34]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.accessibility.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"from":"2024-06-20","to":"2024-12-20","users":[1,2,34],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.accessibility.get
    ```

- JS

    ```js
    BX24.callMethod(
        'calendar.accessibility.get',
        {
            from: '2024-06-20',
            to: '2024-12-20',
            users: [1, 2, 34]
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.accessibility.get',
        [
            'from' => '2024-06-20',
            'to' => '2024-12-20',
            'users' => [1, 2, 34]
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
    "result": {
        "1": [
            {
                "ID": "1213",
                "NAME": "Event name",
                "DATE_FROM": "02.12.2024 11:00:00",
                "DATE_TO": "02.12.2024 12:00:00",
                "DATE_FROM_TS_UTC": "1733158800",
                "DATE_TO_TS_UTC": "1733162400",
                "~USER_OFFSET_FROM": -21600,
                "~USER_OFFSET_TO": -21600,
                "DT_SKIP_TIME": "N",
                "TZ_FROM": "America/Managua",
                "TZ_TO": "America/Managua",
                "ACCESSIBILITY": "busy",
                "IMPORTANCE": "normal",
                "EVENT_TYPE": "#collab#"
            },
            {
                "ID": "1216",
                ...
            }
        ],
        "2": [
            {
                "ID": 1,
                ...
            },
            {
                "ID": 2,
                ...
            }
        ],
        "34": []
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The result contains an object.

The key of the object is the user ID from the request.

The value is an array of objects, each describing an [event](#event) in which the user is busy during the specified period ||
|#

#### Event Object {#event}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Event ID ||
|| **NAME**
[`string`](../data-types.md) | Event name ||
|| **DATE_FROM**
[`datetime`](../data-types.md) | Start date and time of the event ||
|| **DATE_TO**
[`datetime`](../data-types.md) | End date and time of the event ||
|| **DATE_FROM_TS_UTC**
[`string`](../data-types.md) | Start date and time of the event in UTC in timestamp format ||
|| **DATE_TO_TS_UTC**
[`string`](../data-types.md) | End date and time of the event in UTC in timestamp format ||
|| **~USER_OFFSET_FROM**
[`integer`](../data-types.md) | Time offset of the start of the event relative to UTC in seconds ||
|| **~USER_OFFSET_TO**
[`integer`](../data-types.md) | Time offset of the end of the event relative to UTC in seconds ||
|| **DT_SKIP_TIME**
[`integer`](../data-types.md) | Flag indicating that the event lasts all day. Possible values:
- `Y` — all day
- `N` — not all day ||
|| **TZ_FROM**
[`integer`](../data-types.md) | Timezone of the event's start date ||
|| **TZ_TO**
[`integer`](../data-types.md) | Timezone of the event's end date ||
|| **ACCESSIBILITY**
[`integer`](../data-types.md) | Availability of event participants. Possible values:

- `busy` — busy
- `absent` — absent
- `quest` — tentative
- `free` — free ||
|| **IMPORTANCE**
[`string`](../data-types.md) | Importance of the event. Possible values:

- `high` — high
- `normal` — medium
- `low` — low ||
|| **EVENT_TYPE**
[`string`](../data-types.md) | Some events contain information about how they were created.

An event can be created through:

- `#shared#` — calendar slots
- `#shared_crm#` — CRM slots
- `#collab#` — collaboration
- `#shared_collab#` — collaboration slots ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "from" for the method "calendar.accessibility.get" is not set"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "from" for the method "calendar.accessibility.get" is not set | The required parameter `from` is missing ||
|| Empty string | The required parameter "to" for the method "calendar.accessibility.get" is not set | The required parameter `to` is missing ||
|| Empty string | The required parameter "users" for the method "calendar.accessibility.get" is not set | The required parameter `users` is missing ||
|| Empty string | Access denied | Access to the method is prohibited for external users ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)