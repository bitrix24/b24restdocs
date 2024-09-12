# Get Task List task.items.getlist

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method returns an array of tasks, each containing an array of fields (similar to the array returned by [task.item.getdata](task-item/task-item-get-data.md)).

{% note warning %}

Instead of this method, it is recommended to use the methods [`task.item.*`](task-item/index.md).

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **ORDER**
[`object`](../../data-types.md) | An array in the format `{'sorting_field': 'sorting_direction' [, ...]}` for sorting the result. The sorting field can take the following values: 
- `TITLE` — task title  
- `DATE_START` — start date 
- `DEADLINE` — deadline 
- `STATUS` — status 
- `PRIORITY` — priority 
- `MARK` — rating 
- `CREATED_BY` — creator 
- `RESPONSIBLE_ID` — executor 
- `GROUP_ID` — working group 

The sorting direction can take the following values: 
- `asc` — ascending 
- `desc` — descending 
  
Optional parameter. By default, it is sorted in descending order by task ID. 

Sorting by custom fields is allowed 
||
|| **FILTER**
[`object`](../../data-types.md) | An array in the format `{'filter_field': "filter_value" [, ...]}`. The filter field can take the following values: 
- `ID` — task ID
- `PARENT_ID` — parent task ID
- `GROUP_ID` — working group ID
- `CREATED_BY` — creator
- `STATUS_CHANGED_BY` — user who last changed the task status
- `PRIORITY` — priority
- `FORUM_TOPIC_ID` — forum topic ID
- `RESPONSIBLE_ID` — executor
- `TITLE` — task title (can search using the pattern `[%_]`)
- `TAG` — tag
- `REAL_STATUS` — task status with possible values:
    - `STATE_NEW` = 1
    - `STATE_PENDING` = 2
    - `STATE_IN_PROGRESS` = 3
    - `STATE_SUPPOSEDLY_COMPLETED` = 4
    - `STATE_COMPLETED` = 5
    - `STATE_DEFERRED` = 6
    - `STATE_DECLINED` = 7
- `STATUS` — status for sorting. Similar to `REAL_STATUS`, but also includes meta-statuses:
    - `-2` — unread task
    - `-1` — overdue task
- `MARK` — rating
- `XML_ID` — external code
- `SITE_ID` — site ID
- `ADD_IN_REPORT` — task in report (Y\|N)
- `DATE_START` — start date
- `DEADLINE` — deadline
- `CREATED_DATE` — creation date
- `CLOSED_DATE` — completion date
- `CHANGED_DATE` — last modified date
- `ACCOMPLICE` — participant ID
- `AUDITOR` — auditor ID
- `DEPENDS_ON` — ID of the previous task
- `ONLY_ROOT_TASKS` — only tasks that are not subtasks (root tasks), as well as subtasks of the parent task that the current user does not have access to (Y\|N)
- `SUBORDINATE_TASKS` — tasks of the current user and their subordinates (Y\|N)
- `OVERDUED` — were overdue (Y\|N)
- `DEPARTMENT_ID` — department ID

Before the filter field name, you can specify the type of filtering:
- `!` — not equal
- `<` — less than
- `<=` — less than or equal to
- `>` — greater than
- `>=` — greater than or equal to
  
Filter values can be a single value or an array. 

Optional parameter. By default, records are not filtered ||
|| **TASKDATA**
[`array`](../../data-types.md) | Array of returned task fields ||
|| **NAV_PARAMS**
[`array`](../../data-types.md) | Pagination. The option `iNumPage` is available — page number ||
|#

The order of parameters in the request must be followed. If violated, the request will be executed with errors.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Get a list of all tasks (by default, pagination will be applied with 50 items per page).

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.items.getlist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/task.items.getlist?auth=**put_access_token_here**
    ```

- JS

    ```js
    BX24.callMethod(
        'task.items.getlist',
        [],
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.items.getlist',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Get a list of tasks with IDs 1, 2, 3, 4, 5, 6, selecting only the fields `ID` and `TITLE`. Pagination mode — 2 items per page, 2nd page. Sorting by `ID` — descending.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"desc"},"filter":{"ID":[1,2,3,4,5,6]},"select":["ID","TITLE"],"NAV_PARAMS":{"iNumPage":2}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.items.getlist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"desc"},"filter":{"ID":[1,2,3,4,5,6]},"select":["ID","TITLE"],"NAV_PARAMS":{"iNumPage":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.items.getlist
    ```

- JS

    ```js
    BX24.callMethod(
        'task.items.getlist',
        [
            {ID : 'desc'},        // Sorting by ID — descending.
            {ID: [1,2,3,4,5,6]},    // Filter
            ['ID', 'TITLE'],    // Selected fields
            {
                NAV_PARAMS: {        // pagination
                    iNumPage : 2        // page number 2
                }
            }
        ],
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.items.getlist',
        [
            'order' => ['ID' => 'desc'],
            'filter' => ['ID' => [1, 2, 3, 4, 5, 6]],
            'select' => ['ID', 'TITLE'],
            'NAV_PARAMS' => ['iNumPage' => 2]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

To limit the load on pagination, a limit of 50 tasks has been imposed.