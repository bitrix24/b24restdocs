# Get Task List: task.item.list

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method returns an array of tasks, each containing an array of fields (similar to the array returned by [task.item.getdata](task-item-get-data.md)).

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [tasks.task.list](../../tasks-task-list.md).

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **ORDER**
[`object`](../../../data-types.md) | An array in the format `{'sorting_field': 'sorting_direction' [, ...]}` for sorting the results. The sorting field can take the following values: 
- `TITLE` — task title 
- `DATE_START` — start date 
- `DEADLINE` — deadline 
- `STATUS` — status 
- `PRIORITY` — priority 
- `MARK` — rating 
- `CREATED_BY` — Creator 
- `RESPONSIBLE_ID` — Participant 
- `GROUP_ID` — working group 

The sorting direction can take the following values: 
- `asc` — ascending 
- `desc` — descending 
   
This is an optional parameter. By default, it is sorted in descending order by task ID. ||
|| **FILTER**
[`object`](../../../data-types.md) | An array in the format `{'filter_field': 'filter_value' [, ...]}`. The filter field can take the following values:
- `ID` — task ID
- `PARENT_ID` — parent task ID
- `GROUP_ID` — working group ID
- `CREATED_BY` — Creator
- `STATUS_CHANGED_BY` — user who last changed the task status
- `PRIORITY` — priority
- `FORUM_TOPIC_ID` — forum topic ID
- `RESPONSIBLE_ID` — Participant
- `TITLE` — task title (can be searched using the pattern `[%_]`)
- `TAG` — tag
- `REAL_STATUS` — task status. Constants reflecting task statuses:
    - `STATE_NEW` = 1
    - `STATE_PENDING` = 2
    - `STATE_IN_PROGRESS` = 3
    - `STATE_SUPPOSEDLY_COMPLETED` = 4
    - `STATE_COMPLETED` = 5
    - `STATE_DEFERRED` = 6
    - `STATE_DECLINED` = 7
- `STATUS` — status for sorting. Similar to `REAL_STATUS`, but has two additional meta-statuses:
    - `-2` — unread task
    - `-1` — overdue task
- `MARK` — rating
- `SITE_ID` — site ID
- `ADD_IN_REPORT` — task in report (Y\|N)
- `DATE_START` — start date
- `DEADLINE` — deadline
- `CREATED_DATE` — creation date
- `CLOSED_DATE` — completion date
- `CHANGED_DATE` — last modification date
- `ACCOMPLICE` — co-participant ID
- `AUDITOR` — auditor ID
- `DEPENDS_ON` — ID of the preceding task
- `ONLY_ROOT_TASKS` — only tasks that are not sub-tasks (root tasks), as well as sub-tasks of the parent task to which the current user does not have access (Y\|N)

A filter type can be specified before the filter field name:
- `!` — not equal
- `<` — less than
- `<=` — less than or equal to
- `>` — greater than
- `>=` — greater than or equal to

Filter values can be a single value or an array.

This is an optional parameter. By default, records are not filtered.

For the method `task.item.list`, sorting must be specified for filtering. Filtering without sorting returns all tasks.
||
|| **PARAMS**
[`array`](../../../data-types.md) | An array for call options. An element is an array `NAV_PARAMS` in the format `{'call_option': 'value' [, ...]}`, containing the following options:
- `nPageSize` — number of items per page. A limit of 50 tasks has been imposed for pagination to reduce load.
- `iNumPage` — page number for pagination. ||
|| **SELECT**
[`array`](../../../data-types.md) | An array of record fields that will be returned by the method. If the array contains the value `"*"`, all available fields will be returned. 

The default value (an empty array `array()`) means that all fields from the main query table will be returned. ||
|#

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

However, if some parameters need to be skipped, they still need to be passed, but as empty arrays: `ORDER[]=&FILTER[]=&PARAMS[]=&SELECT[]=`.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Get a list of all tasks (pagination will default to 50 items per page).

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/task.item.list?auth=**put_access_token_here**
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory load.
    
    try {
      const response = await $b24.callListMethod(
        'task.item.list',
        {},
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use for large volumes of data for efficient memory consumption.
    
    try {
      const generator = $b24.fetchListMethod('task.item.list', {}, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('task.item.list', {}, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.item.list',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Data: ' . print_r($result, true);
        echo 'Full Result: ' . print_r($response, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching task list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.item.list',
        [],
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.item.list',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Get a list of tasks with IDs 1, 2, 3, 4, 5, 6, selecting only the fields `ID` and `TITLE`. Pagination mode — 2 items per page, 2nd page. Sorting by ID — descending.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"desc"},"filter":{"ID":[1,2,3,4,5,6]},"params":{"NAV_PARAMS":{"nPageSize":2,"iNumPage":2}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"desc"},"filter":{"ID":[1,2,3,4,5,6]},"params":{"NAV_PARAMS":{"nPageSize":2,"iNumPage":2}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.item.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory load.
    
    try {
      const response = await $b24.callListMethod(
        'task.item.list',
        [
          {ID: 'desc'},        // Sorting by ID — descending.
          {ID: [1, 2, 3, 4, 5, 6]},    // Filter
          {
            NAV_PARAMS: { // pagination
              nPageSize: 2,    // 2 items per page.
              iNumPage: 2    // page number 2        
            }
          }
        ],
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use for large volumes of data for efficient memory consumption.
    
    try {
      const generator = $b24.fetchListMethod('task.item.list', [
        {ID: 'desc'},        // Sorting by ID — descending.
        {ID: [1, 2, 3, 4, 5, 6]},    // Filter
        {
          NAV_PARAMS: { // pagination
            nPageSize: 2,    // 2 items per page.
            iNumPage: 2    // page number 2        
          }
        }
      ], 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('task.item.list', [
        {ID: 'desc'},        // Sorting by ID — descending.
        {ID: [1, 2, 3, 4, 5, 6]},    // Filter
        {
          NAV_PARAMS: { // pagination
            nPageSize: 2,    // 2 items per page.
            iNumPage: 2    // page number 2        
          }
        }
      ], 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.item.list',
                [
                    ['ID' => 'desc'], // Sorting by ID — descending.
                    ['ID' => [1, 2, 3, 4, 5, 6]], // Filter
                    [
                        'NAV_PARAMS' => [ // pagination
                            'nPageSize' => 2, // 2 items per page.
                            'iNumPage'  => 2 // page number 2
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching task list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.item.list',
        [
            {ID : 'desc'},        // Sorting by ID — descending.
            {ID: [1,2,3,4,5,6]},    // Filter
            {    
                NAV_PARAMS: { // pagination
                    nPageSize : 2,    // 2 items per page.
                    iNumPage : 2    // page number 2        
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.item.list',
        [
            'order' => ['ID' => 'desc'],
            'filter' => ['ID' => [1, 2, 3, 4, 5, 6]],
            'params' => [
                'NAV_PARAMS' => [
                    'nPageSize' => 2,
                    'iNumPage' => 2
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}