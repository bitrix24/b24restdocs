# Get Flow Team tasks.flow.Team.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: a user who can see the flow — a Bitrix24 administrator, a flow member, or an employee of a department specified in the flow. If all users can add tasks to the flow, all employees can see it

The method `tasks.flow.Team.list` returns the employees of the flow team.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **flowData***
[`object`](../../data-types.md) | An object with the flow identifier in the `id` field, for example `{"id": 517}`.

The identifier is returned by the [tasks.flow.Flow.create](./tasks-flow-flow-create.md) method when a flow is created, and for a task from a flow — by the [tasks.task.get](../tasks-task-get.md) method ||
|| **withNavigation**
[`boolean`](../../data-types.md) | Whether to return pagination data together with the list.

Possible values:

- `true` — the method returns an object with the `members` and `navigation` fields
- `false` — the method returns an array of employees

The `true` value can be passed as `true`, `1`, `"1"`, `"true"`, or `"Y"`; all other values are treated as `false`.

Default: `false` ||
|| **start**
[`integer`](../../data-types.md) | Offset from the beginning of the list. The method returns 50 employees per request. To retrieve the next page, pass `start` = (`nextPage` − 1) × 50, where `nextPage` is the next page number from a response with `withNavigation`.

Default: `0` ||
|#

The flow identifier is passed only inside `flowData`. The method does not accept the `flowId` parameter, as in [tasks.flow.Flow.get](./tasks-flow-flow-get.md), and responds with the `Unknown error. [Team: 97]` error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "flowData": {
            "id": 517
        },
        "withNavigation": true,
        "start": 0
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.flow.Team.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "flowData": {
            "id": 517
        },
        "withNavigation": true,
        "start": 0,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.flow.Team.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FlowTeamMember = {
      id: string,
      name: string,
      avatar: string,
      workPosition: string,
      pathToProfile: string,
    }

    type FlowTeamListResult = {
      members: FlowTeamMember[],
      navigation: {
        page: number,
        pageSize: number,
        hasMore: boolean,
        nextPage: number | null,
      },
    }

    try {
      const response = await $b24.actions.v2.call.make<FlowTeamListResult>({
        method: 'tasks.flow.Team.list',
        params: {
          flowData: { id: 517 },
          withNavigation: true,
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.members.forEach((member) => console.info(member.id, member.name))
        console.info('Next page:', result.navigation.nextPage)
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
      async function getFlowTeam() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.flow.Team.list',
            params: {
              flowData: { id: 517 },
              withNavigation: true,
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.members.forEach((member) => console.info(member.id, member.name))
          console.info('Next page:', result.navigation.nextPage)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFlowTeam)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        response = bitrix_token.call_method(
            api_method="tasks.flow.Team.list",
            params={
                "flowData": {"id": 517},
                "withNavigation": True,
                "start": 0,
            },
        )
        result = response["result"]
        for member in result["members"]:
            print(member["id"], member["name"])
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
                'tasks.flow.Team.list',
                [
                    'flowData' => ['id' => 517],
                    'withNavigation' => true,
                    'start' => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.flow.Team.list',
        {
            flowData: { id: 517 },
            withNavigation: true,
            start: 0
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

- PHP CRest

    ```php
    require_once('crest.php'); // connecting the CRest PHP SDK

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.flow.Team.list',
        [
            'flowData' => ['id' => 517],
            'withNavigation' => true,
            'start' => 0,
        ]
    );

    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.flow.Team.list", b24.Params{
    	"flowData":       b24.Params{"id": 517},
    	"withNavigation": true,
    	"start":          0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.flow.Team.list: %w", err)
    }

    var page struct {
    	Members []struct {
    		ID            string `json:"id"`
    		Name          string `json:"name"`
    		WorkPosition  string `json:"workPosition"`
    		PathToProfile string `json:"pathToProfile"`
    	} `json:"members"`
    	Navigation struct {
    		Page     int  `json:"page"`
    		PageSize int  `json:"pageSize"`
    		HasMore  bool `json:"hasMore"`
    		NextPage *int `json:"nextPage"`
    	} `json:"navigation"`
    }
    if err := json.Unmarshal(res.Result, &page); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, member := range page.Members {
    	fmt.Println(member.ID, member.Name)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

Response with `withNavigation: true`:

```json
{
    "result": {
        "members": [
            {
                "id": "1",
                "name": "Maria Fischer",
                "avatar": "https://example.bitrix24.com/upload/resize_cache/main/avatar.jpg",
                "workPosition": "Head of Sales",
                "pathToProfile": "/company/personal/user/1/"
            },
            {
                "id": "3",
                "name": "Klaus Weber",
                "avatar": "",
                "workPosition": "Sales Manager",
                "pathToProfile": "/company/personal/user/3/"
            }
        ],
        "navigation": {
            "page": 1,
            "pageSize": 50,
            "hasMore": false,
            "nextPage": null
        }
    },
    "time": {
        "start": 1795439790,
        "finish": 1795439790.241665,
        "duration": 0.24166488647460938,
        "processing": 0,
        "date_start": "2026-11-23T14:16:30+01:00",
        "date_finish": "2026-11-23T14:16:30+01:00"
    }
}
```

Without `withNavigation`, `result` contains only an array of employees, as in the `members` field. If the flow team is empty or the page is outside the list, the array is empty.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Without `withNavigation` — an array of team employees [(detailed description)](#member).

With `withNavigation: true` — an object with the `members` and `navigation` fields [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **members**
[`array`](../../data-types.md) | Team employees on the current page [(detailed description)](#member) ||
|| **navigation**
[`object`](../../data-types.md) | Pagination data [(detailed description)](#navigation) ||
|#

#### Team Employee {#member}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Employee identifier ||
|| **name**
[`string`](../../data-types.md) | Employee's first and last name ||
|| **avatar**
[`string`](../../data-types.md) | Link to the employee's photo. An empty string if there is no photo ||
|| **workPosition**
[`string`](../../data-types.md) | Employee's position. An empty string if no position is specified ||
|| **pathToProfile**
[`string`](../../data-types.md) | Relative path to the employee's profile ||
|#

#### Object navigation {#navigation}

#|
|| **Name**
`type` | **Description** ||
|| **page**
[`integer`](../../data-types.md) | Current page number ||
|| **pageSize**
[`integer`](../../data-types.md) | Page size. Always `50` via REST ||
|| **hasMore**
[`boolean`](../../data-types.md) | Whether there is a next page ||
|| **nextPage**
[`integer`](../../data-types.md) | Next page number. `null` if there is no next page ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Cannot find flow"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Cannot find flow | The user does not have permission to view the flow ||
|| `400` | `0` | Unknown error. [Team: 97] | `flowData` is not passed, or `flowData.id` contains a nonexistent flow or a non-numeric value. The method also responds this way if `flowId` is passed instead of `flowData` ||
|| `400` | `0` | Internal server error. | `flowData` is not passed as an object, for example, as the number `517` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-flow-flow-get.md)
- [{#T}](./tasks-flow-flow-create.md)
- [{#T}](./tasks-flow-flow-update.md)
