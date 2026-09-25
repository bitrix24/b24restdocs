# Get a List of Time Tracking Records task.elapseditem.getlist

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to the task

The method `task.elapseditem.getlist` returns a list of time tracking records for a task.

## Method Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **taskId**
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [get task list method](../tasks-task-list.md) ||
|| **order**
[`object`](../../data-types.md) | Object for sorting the result [(detailed description)](#order) ||
|| **filter**
[`object`](../../data-types.md) | Object for filtering the result [(detailed description)](#filter) ||
|| **select**
[`array`](../../data-types.md) | Array of record fields returned by the method [(detailed description)](#select)

If this parameter is not passed, the method returns all fields of the main request table. To explicitly return all available fields, pass `*` ||
|| **params**
[`object`](../../data-types.md) | Object containing call options [(detailed description)](#params) ||
|#

{% note warning "" %}

The method accepts parameters positionally. Follow the order from the table: `taskId`, `order`, `filter`, `select`, `params`. If you pass `order`, `filter`, `select`, and `params` as named fields of a single object, the request will fail.

{% endnote %}

{% note info "" %}

Features of manually adding information about work time that was actually performed several days ago. In this case, the values of some fields change:
- `CREATED_DATE` — start date
- `DATE_START` — record creation date
- `DATE_STOP` — record end date

{% endnote %}

### order Parameter {#order}

#| 
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Identifier of the time tracking record. Can take values:
- `asc` — ascending
- `desc` — descending ||
|| **TASK_ID**
[`string`](../../data-types.md) | Task identifier. Possible values:
- `asc` — ascending
- `desc` — descending ||
|| **USER_ID**
[`string`](../../data-types.md) | Identifier of the user on behalf of whom the time tracking record was made. Can take values:
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

### filter Parameter {#filter}

#| 
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the time tracking record ||
|| **TASK_ID**
[`integer`](../../data-types.md) | Task identifier ||
|| **USER_ID**
[`integer`](../../data-types.md) | Identifier of the user on behalf of whom the time tracking record was made ||
|| **CREATED_DATE**
[`datetime`](../../data-types.md) | Record creation date ||
|#

{% note info "" %}

Before the name of the filtered field, you can specify the type of filtering:
- "!" — not equal
- "<" — less than
- "<=" — less than or equal
- ">" — greater than
- ">=" — greater than or equal

*'filter values'* — a single value or an array

{% endnote %}

### select Parameter {#select}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Time entry identifier ||
|| **TASK_ID**
[`string`](../../data-types.md) | Task identifier ||
|| **USER_ID**
[`string`](../../data-types.md) | Entry author identifier ||
|| **COMMENT_TEXT**
[`string`](../../data-types.md) | Comment ||
|| **SECONDS**
[`string`](../../data-types.md) | Time spent in seconds ||
|| **MINUTES**
[`string`](../../data-types.md) | Time spent in minutes ||
|| **SOURCE**
[`string`](../../data-types.md) | Entry source ||
|| **CREATED_DATE**
[`string`](../../data-types.md) | Entry creation date ||
|| **DATE_START**
[`string`](../../data-types.md) | Date and time when tracking started ||
|| **DATE_STOP**
[`string`](../../data-types.md) | Date and time when tracking ended ||
|#

### params Parameter {#params}

#|
|| **Name**
`type` | **Description** ||
|| **NAV_PARAMS**
[`object`](../../data-types.md) | Pagination settings [(detailed description)](#nav-params) ||
|#

#### NAV_PARAMS Object {#nav-params}

#| 
|| **Name**
`type` | **Description** ||
|| **nPageSize**
[`integer`](../../data-types.md) | Number of items per page. To limit the load on pagination, a limit of 50 records is imposed ||
|| **iNumPage**
[`integer`](../../data-types.md) | Page number in pagination ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '[3839,{"ID":"asc"},{"USER_ID":1},["ID","TASK_ID","USER_ID","SECONDS","MINUTES","COMMENT_TEXT","CREATED_DATE"],{"NAV_PARAMS":{"nPageSize":2,"iNumPage":1}}]' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.elapseditem.getlist
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '[3839,{"ID":"asc"},{"USER_ID":1},["ID","TASK_ID","USER_ID","SECONDS","MINUTES","COMMENT_TEXT","CREATED_DATE"],{"NAV_PARAMS":{"nPageSize":2,"iNumPage":1}}]' \
    https://**put_your_bitrix24_address**/rest/task.elapseditem.getlist?auth=**put_access_token_here**
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ElapsedItem = {
      ID: string
      TASK_ID: string
      USER_ID: string
      COMMENT_TEXT: string
      SECONDS: string
      MINUTES: string
      SOURCE: string
      CREATED_DATE: ISODate | null
      DATE_START: ISODate | null
      DATE_STOP: ISODate | null
    }

    try {
      // task.elapseditem.getlist returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make variant when sort matters.
      const response = await $b24.actions.v2.call.make<ElapsedItem[]>({
        method: 'task.elapseditem.getlist',
        params: [
          3839,
          { ID: 'asc' },
          { USER_ID: 1 },
          ['ID', 'TASK_ID', 'USER_ID', 'SECONDS', 'MINUTES', 'COMMENT_TEXT', 'CREATED_DATE'],
          { NAV_PARAMS: { nPageSize: 2, iNumPage: 1 } },
        ],
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Elapsed items count:', result.length, 'First item:', result[0])
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
      async function getElapsedItems() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // task.elapseditem.getlist returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'task.elapseditem.getlist',
            params: [
              3839,
              { ID: 'asc' },
              { USER_ID: 1 },
              ['ID', 'TASK_ID', 'USER_ID', 'SECONDS', 'MINUTES', 'COMMENT_TEXT', 'CREATED_DATE'],
              { NAV_PARAMS: { nPageSize: 2, iNumPage: 1 } },
            ],
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Elapsed items count:', result.length, 'First item:', result[0])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getElapsedItems)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    order = {
        "ID": "asc",
    }

    filter = {
        "USER_ID": 1,
    }

    try:
        bitrix_response = client.task.elapseditem.getlist(
            taskid=3839,
            order=order,
            filter=filter,
            select=["ID", "TASK_ID", "USER_ID", "SECONDS", "MINUTES", "COMMENT_TEXT", "CREATED_DATE"],
            params={
                "NAV_PARAMS": {
                    "nPageSize": 2,
                    "iNumPage": 1,
                },
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```
- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.elapseditem.getlist',
                [
                    3839,
                    ['ID' => 'asc'],
                    ['USER_ID' => 1],
                    ['ID', 'TASK_ID', 'USER_ID', 'SECONDS', 'MINUTES', 'COMMENT_TEXT', 'CREATED_DATE'],
                    [
                        'NAV_PARAMS' => [
                            'nPageSize' => 2,
                            'iNumPage' => 1,
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting elapsed time records: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.elapseditem.getlist',
        [
            3839,
            {'ID': 'asc'},
            {'USER_ID': 1},
            ['ID', 'TASK_ID', 'USER_ID', 'SECONDS', 'MINUTES', 'COMMENT_TEXT', 'CREATED_DATE'],
            {"NAV_PARAMS":{
                    "nPageSize":2,
                    "iNumPage":1
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.elapseditem.getlist',
        [
            3839,
            ['ID' => 'asc'],
            ['USER_ID' => 1],
            ['ID', 'TASK_ID', 'USER_ID', 'SECONDS', 'MINUTES', 'COMMENT_TEXT', 'CREATED_DATE'],
            [
                'NAV_PARAMS' => [
                    'nPageSize' => 2,
                    'iNumPage' => 1,
                ],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "task.elapseditem.getlist", []any{
        3839,
        b24.Params{
            "ID": "asc",
        },
        b24.Params{
            "USER_ID": 1,
        },
        []string{"ID", "TASK_ID", "USER_ID", "SECONDS", "MINUTES", "COMMENT_TEXT", "CREATED_DATE"},
        b24.Params{
            "NAV_PARAMS": b24.Params{
                "nPageSize": 2,
                "iNumPage": 1,
            },
        },
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("task.elapseditem.getlist: %w", err)
    }

    var items []struct {
    	ID          b24.ID `json:"ID"`
    	TaskID      b24.ID `json:"TASK_ID"`
    	UserID      b24.ID `json:"USER_ID"`
    	CommentText string `json:"COMMENT_TEXT"`
    	Seconds     string `json:"SECONDS"`
    	Minutes     string `json:"MINUTES"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.TaskID)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result":[
        {
            "ID": "153",
            "TASK_ID": "3839",
            "USER_ID": "1",
            "COMMENT_TEXT": "",
            "SECONDS": "5100",
            "MINUTES": "85",
            "CREATED_DATE": "2025-12-18T14:16:51+02:00"
        },
        {
            "ID": "155",
            "TASK_ID": "3839",
            "USER_ID": "1",
            "COMMENT_TEXT": "",
            "SECONDS": "23",
            "MINUTES": "0",
            "CREATED_DATE": "2025-12-18T14:16:37+02:00"
        }
    ],
    "total": 2,
    "time":{
        "start":1787829762,
        "finish":1787829762.985642,
        "duration":0.9856419563293457,
        "processing":0,
        "date_start":"2026-08-27T14:22:42+02:00",
        "date_finish":"2026-08-27T14:22:42+02:00",
        "operating_reset_at":1787830362,
        "operating":0.11980605125427246
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of objects containing information about time entries for the task [(detailed description)](#result) ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### result[] Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Time entry identifier ||
|| **TASK_ID**
[`string`](../../data-types.md) | Task identifier ||
|| **USER_ID**
[`string`](../../data-types.md) | Entry author identifier ||
|| **COMMENT_TEXT**
[`string`](../../data-types.md) | Comment ||
|| **SECONDS**
[`string`](../../data-types.md) | Time spent in seconds ||
|| **MINUTES**
[`string`](../../data-types.md) | Time spent in minutes ||
|| **SOURCE**
[`string`](../../data-types.md) | Entry source:
- `1` — source is not specified
- `2` — entry was added manually
- `3` — entry was added automatically ||
|| **CREATED_DATE**
[`datetime`](../../data-types.md) | Entry creation date ||
|| **DATE_START**
[`datetime`](../../data-types.md) | Date and time when tracking started ||
|| **DATE_STOP**
[`datetime`](../../data-types.md) | Date and time when tracking ended ||
|#

The fields in the `result[]` objects depend on the `select` parameter.

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
|| **Code** | **Internal Code** | **Description** ||
|| `ERROR_CORE` | `0x000001` | The task was not found or is unavailable if `taskId` was passed ||
|| `ERROR_CORE` | `0x000100` | The parameter order is invalid, an invalid type was specified, or an unsupported field or value was passed ||
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
