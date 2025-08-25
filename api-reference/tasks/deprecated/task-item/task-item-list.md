# Get Task List task.item.list

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns an array of tasks, each containing an array of fields (similar to the array returned by [task.item.getdata](task-item-get-data.md)).

{% note warning %}

The method is deprecated and not supported. It is recommended to use the methods [tasks.task.*](../../index.md).

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **ORDER**
[`object`](../../../data-types.md) | An array of the form `{'sorting_field': 'sorting_direction' [, ...]}` for sorting the result. The sorting field can take the following values: 
- `TITLE` — task title 
- `DATE_START` — start date 
- `DEADLINE` — deadline 
- `STATUS` — status 
- `PRIORITY` — priority 
- `MARK` — rating 
- `CREATED_BY` — creator 
- `RESPONSIBLE_ID` — assignee 
- `GROUP_ID` — workgroup 

The sorting direction can take the following values: 
- `asc` — ascending 
- `desc` — descending 
   
Optional parameter. By default, it is sorted in descending order by task ID ||
|| **FILTER**
[`object`](../../../data-types.md) | An array of the form `{'filter_field': 'filter_value' [, ...]}`. The filter field can take the following values:
- `ID` — task ID
- `PARENT_ID` — parent task ID
- `GROUP_ID` — workgroup ID
- `CREATED_BY` — creator
- `STATUS_CHANGED_BY` — user who last changed the task status
- `PRIORITY` — priority
- `FORUM_TOPIC_ID` — forum topic ID
- `RESPONSIBLE_ID` — assignee
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
- `ACCOMPLICE` — participant ID
- `AUDITOR` — auditor ID
- `DEPENDS_ON` — ID of the previous task
- `ONLY_ROOT_TASKS` — only tasks that are not subtasks (root tasks), as well as subtasks of the parent task to which the current user does not have access (Y\|N)

Before the name of the filter field, you can specify the type of filtering:
- `!` — not equal
- `<` — less than
- `<=` — less than or equal to
- `>` — greater than
- `>=` — greater than or equal to

Filter values can be a single value or an array.

Optional parameter. By default, records are not filtered.

For the `task.item.list` method, sorting must be specified for filtering. Filtering without sorting returns all tasks
||
|| **PARAMS**
[`array`](../../../data-types.md) | An array for call options. An element is an array `NAV_PARAMS` of the form `{'call_option': 'value' [, ...]}`, storing the following options:
- `nPageSize` — number of items per page. To limit the load on pagination, a limit of 50 tasks is imposed
- `iNumPage` — page number for pagination ||
|| **SELECT**
[`array`](../../../data-types.md) | An array of record fields that will be returned by the method. If the array contains the value `"*"`, all available fields will be returned. 

The default value (empty array `array()`) means that all fields of the main query table will be returned ||
|#

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

However, if some parameters need to be skipped, they still need to be passed, but as empty arrays: `ORDER[]=&FILTER[]=&PARAMS[]=&SELECT[]=`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Get a list of all tasks (by default, pagination will be applied — 50 items per page).

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
    // callListMethod is recommended when you need to get the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
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
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('task.item.list', {}, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination data retrieval process via the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
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
    // callListMethod is recommended when you need to get the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
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
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
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
    
    // callMethod provides manual control over the pagination data retrieval process via the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
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
        // Your logic for processing data
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