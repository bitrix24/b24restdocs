# Get a List of Time Spent Records task.elapseditem.getlist

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of time spent records for a task.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **TASKID**
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [get task list method](../tasks-task-list.md) ||
|| **ORDER**
[`object`](../../data-types.md) | An object for sorting the result (detailed description provided below) ||
|| **FILTER**
[`object`](../../data-types.md) | An object for filtering the result (detailed description provided below) ||
|| **SELECT**
[`array`](../../data-types.md) | An array of fields from the records that will be returned by the method. You can specify only the fields that are necessary. If the array contains the value `"*"`, all available fields will be returned.

By default, all fields from the main query table will be returned ||
|| **PARAMS**
[`object`](../../data-types.md) | An object for call options. The element is an object `NAV_PARAMS` of the form `{'call option': 'value' [, ...]}` (detailed description provided below) in structure ||
|#

{% note warning %}

It is mandatory to follow the specified order of parameters in the request as shown in the table. Otherwise, the request will execute with errors.

{% endnote %}


{% note info %}

Features of manually adding information about work time that was actually performed several days ago. In this case, the values of some fields change:
- `CREATED_DATE` — start date
- `DATE_START` — record creation date
- `DATE_STOP` — record end date

{% endnote %}

### ORDER Parameter

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Identifier of the time spent record. Can take values:
- `asc` — ascending
- `desc` — descending ||
|| **USER_ID**
[`string`](../../data-types.md) | Identifier of the user on behalf of whom the time spent record was made. Can take values:
- `asc` — ascending
- `desc` — descending ||
|| **MINUTES**
[`string`](../../data-types.md) | Time spent, in minutes. Can take values:
- `asc` — ascending
- `desc` — descending ||
|| **SECONDS**
[`string`](../../data-types.md) | Time spent, in seconds. Can take values:
- `asc` — ascending
- `desc` — descending ||
|| **CREATED_DATE**
[`string`](../../data-types.md) | Record creation date. Can take values:
- `asc` — ascending
- `desc` — descending ||
|| **DATE_START**
[`string`](../../data-types.md) | Start date. Can take values:
- `asc` — ascending
- `desc` — descending ||
|| **DATE_STOP**
[`string`](../../data-types.md) | End date. Can take values:
- `asc` — ascending
- `desc` — descending ||
|#


### FILTER Parameter

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the time spent record ||
|| **USER_ID**
[`integer`](../../data-types.md) | Identifier of the user on behalf of whom the time spent record was made ||
|| **CREATED_DATE**
[`datetime`](../../data-types.md) | Record creation date ||
|#

{% note info %}

Before the name of the filtered field, you can specify the type of filtering:
- "!" — not equal
- "<" — less than
- "<=" — less than or equal to
- ">" — greater than
- ">=" — greater than or equal to

*'filter values'* — a single value or an array

{% endnote %}

### NAV_PARAMS Parameter

#|
|| **Name**
`type` | **Description** ||
|| **nPageSize**
[`integer`](../../data-types.md) | Number of items per page. To limit the load on pagination, a limit of 50 records is imposed ||
|| **iNumPage**
[`integer`](../../data-types.md) | Page number in pagination ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{[{"ID": "desc"},{">=CREATED_DATE": "2024-02-16"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.elapseditem.getlist
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{[{"ID": "desc"},{">=CREATED_DATE": "2024-02-16"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.elapseditem.getlist
    ```

- JS

    ```js
    // Get all time spent records sorted by ID in descending order.
    // Only records with ID less than 50 will be filtered.
    BX24.callMethod(
        'task.elapseditem.getlist',
        [
            1, 
            {'ID': 'desc'},
            {'<ID': 50}
        ],
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    // Retrieve a sample of time spent based on general filtering conditions. For example, select data on labor costs from a specified date:
    BX24.callMethod(
        'task.elapseditem.getlist',
        [
            {'ID': 'desc'}, 
            {'>=CREATED_DATE': '2024-02-16'},
            ['ID', 'TASK_ID'],
            {"NAV_PARAMS":{
                    "nPageSize":2
                }
            },
        ],
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
        'task.elapseditem.getlist',
        [
            "ORDER" => ["ID" => "DESC"],            // Sorting by ID - descending
            "FILTER" => [">ID" => 1],               // Filter
            "SELECT" => ['ID', 'TASK_ID'],          // Selection - only ID of the record and task
            "PARAMS" => ['NAV_PARAMS' => [          // Pagination
                    "nPageSize" => 2,                   // 2 items per page
                    'iNumPage' => 2                     // page number 2
                ]
            ],
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
    "result":[
        {
            "ID": "1",
            "TASK_ID": "691",
            "USER_ID": "1",
            "COMMENT_TEXT": "1",
            "SECONDS": "3600",
            "MINUTES": "60",
            "SOURCE": "2",
            "CREATED_DATE": "2024-05-16T10:33:00+02:00",
            "DATE_START": "2024-05-16T10:33:15+02:00",
            "DATE_STOP": "2024-05-16T10:33:15+02:00"
        }
    ],
    "total": 1,
    "time":{
        "start":1712137817.343984,
        "finish":1712137817.605804,
        "duration":0.26182007789611816,
        "processing":0.018325090408325195,
        "date_start":"2024-04-03T12:50:17+03:00",
        "date_finish":"2024-04-03T12:50:17+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | An array of objects with information about time spent records for the task ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"ACTION_NOT_ALLOWED"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0x100002` | Access denied ||
|| `0x000004` | Action not allowed ||
|| `0x000040` | Unknown error ||
|| `0x000100` | Invalid method parameters provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-elapsed-item-add.md)
- [{#T}](./task-elapsed-item-update.md)
- [{#T}](./task-elapsed-item-get.md)
- [{#T}](./task-elapsed-item-delete.md)
- [{#T}](./task-elapsed-item-is-action-allowed.md)
- [{#T}](./task-elapsed-item-get-manifest.md)