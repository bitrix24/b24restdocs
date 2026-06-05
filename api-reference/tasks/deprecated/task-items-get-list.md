# Get the List of Tasks task.items.getlist

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

Development of this method has been discontinued. Use [tasks.task.list](../tasks-task-list.md).

{% endnote %}

The method returns an array of tasks, each containing an array of fields (similar to the array returned by [task.item.getdata](task-item/task-item-get-data.md)).

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **ORDER**
[`object`](../../data-types.md) | An array in the form `{'sorting_field': 'sorting_direction' [, ...]}` for sorting the result. The sorting field can take the following values: 
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
   
Optional parameter. By default, it is sorted in descending order by task ID. 

Sorting by custom fields is allowed 
||
|| **FILTER**
[`object`](../../data-types.md) | An array in the form `{'filter_field': "filter_value" [, ...]}`. The filter field can take the following values: 
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
- `CHANGED_DATE` — last modification date
- `ACCOMPLICE` — co-participant ID
- `AUDITOR` — auditor ID
- `DEPENDS_ON` — previous task ID
- `ONLY_ROOT_TASKS` — only tasks that are not sub-tasks (root tasks), as well as sub-tasks of the parent task to which the current user does not have access (Y\|N)
- `SUBORDINATE_TASKS` — tasks of the current user and their subordinates (Y\|N)
- `OVERDUED` — were overdue (Y\|N)
- `DEPARTMENT_ID` — department ID

You can specify the type of filtering before the filter field name:
- `!` — not equal
- `<` — less than
- `<=` — less than or equal to
- `>` — greater than
- `>=` — greater than or equal to
  
Filter values can be a single value or an array. 

Optional parameter. By default, records are not filtered ||
|| **TASKDATA**
[`array`](../../data-types.md) | An array of returned task fields ||
|| **NAV_PARAMS**
[`array`](../../data-types.md) | Pagination. The option `iNumPage` is available — page number ||
|#

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Get a list of all tasks (by default, pagination will be applied with a limit of 50 items per page).

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // TODO: verify API version — no response shape defined on this page
    type TaskItem = Record<string, unknown>

    try {
      // task.items.getlist returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<TaskItem[]>({
        method: 'task.items.getlist',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Tasks retrieved:', result.length)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getTaskItemsList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // task.items.getlist returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'task.items.getlist',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Tasks retrieved:', result.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTaskItemsList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.items.getlist',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting task items list: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // TODO: verify API version — no response shape defined on this page
    type TaskItem = Record<string, unknown>

    try {
      // task.items.getlist returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<TaskItem[]>({
        method: 'task.items.getlist',
        params: {
          order: { ID: 'desc' },               // sort by ID descending
          filter: { ID: [1, 2, 3, 4, 5, 6] },  // filter by IDs
          select: ['ID', 'TITLE'],              // return only these fields
          NAV_PARAMS: { iNumPage: 2 },          // page 2
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Tasks on page:', result.length)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getTaskItemsListFiltered() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // task.items.getlist returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'task.items.getlist',
            params: {
              order: { ID: 'desc' },               // sort by ID descending
              filter: { ID: [1, 2, 3, 4, 5, 6] },  // filter by IDs
              select: ['ID', 'TITLE'],              // return only these fields
              NAV_PARAMS: { iNumPage: 2 },          // page 2
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Tasks on page:', result.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTaskItemsListFiltered)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.items.getlist',
                [
                    ['ID' => 'desc'], // Sort by ID — descending.
                    ['ID' => [1, 2, 3, 4, 5, 6]], // Filter
                    ['ID', 'TITLE'], // Selected fields
                    [
                        'NAV_PARAMS' => [ // pagination
                            'iNumPage' => 2 // page number 2
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
        echo 'Error getting task items list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.items.getlist',
        [
            {ID : 'desc'},        // Sort by ID — descending.
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

- PHP CRest

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