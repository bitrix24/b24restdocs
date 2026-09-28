# Get Sprint by Identifier tasks.api.scrum.sprint.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the Scrum

The method `tasks.api.scrum.sprint.get` returns sprint data by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | The identifier of the sprint. 

The identifier can be obtained using the method [tasks.api.scrum.sprint.list](./tasks-api-scrum-sprint-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 2
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 2,
    "auth": "YOUR_ACCESS_TOKEN"
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.get
    ```    

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SprintGetResult = {
      id: number
      groupId: number
      entityType: string
      name: string
      goal: string
      sort: number
      createdBy: number
      modifiedBy: number
      dateStart: ISODate | null
      dateEnd: ISODate | null
      status: string
    }

    try {
      const response = await $b24.actions.v2.call.make<SprintGetResult>({
        method: 'tasks.api.scrum.sprint.get',
        params: {
          id: 2,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.id, result.name, result.status)
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
      async function getSprintById() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.api.scrum.sprint.get',
            params: {
              id: 2,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.id, result.name, result.status)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSprintById)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.api.scrum.sprint.get(
            sprint_id=2,
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
        $sprintId = 2;
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.sprint.get',
                [
                    'id' => $sprintId,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting sprint data: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const sprintId = 2;
    BX24.callMethod(
        'tasks.api.scrum.sprint.get',
        {
            id: sprintId,
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php'); // connecting the CRest PHP SDK

    $sprintId = 2;

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.get',
        [
            'id' => $sprintId
        ]
    );

    // Processing the response from Bitrix24
    if (isset($result['error'])) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 2,
        "groupId": 143,
        "entityType": "sprint",
        "name": "Sprint 1",
        "goal": "",
        "sort": 1,
        "createdBy": 1,
        "modifiedBy": 1,
        "dateStart": "2024-07-19T15:03:01+00:00",
        "dateEnd": "2024-08-02T15:03:01+00:00",
        "status": "planned"
    },
    "time": {
        "start": 1790580587,
        "finish": 1790580587.569156,
        "duration": 0.5691559314727783,
        "processing": 0,
        "date_start": "2026-09-28T10:29:47+03:00",
        "date_finish": "2026-09-28T10:29:47+03:00",
        "operating_reset_at": 1790581187,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`object`](../../../data-types.md) | Sprint data [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier of the sprint ||
|| **groupId** 
[`integer`](../../../data-types.md) | Identifier of the group (Scrum) to which the sprint belongs ||
|| **entityType** 
[`string`](../../../data-types.md) | Object type, always `sprint` for sprints ||
|| **name** 
[`string`](../../../data-types.md) | Name of the sprint ||
|| **goal** 
[`string`](../../../data-types.md) | Goal of the sprint. Set only in the interface when starting the sprint ||
|| **sort** 
[`integer`](../../../data-types.md) | Sort order of the sprint ||
|| **createdBy** 
[`integer`](../../../data-types.md) | Identifier of the user who created the sprint ||
|| **modifiedBy** 
[`integer`](../../../data-types.md) | Identifier of the user who modified the sprint ||
|| **dateStart** 
[`string`](../../../data-types.md) | Start date of the sprint in `ISO 8601` format ||
|| **dateEnd** 
[`string`](../../../data-types.md) | End date of the sprint in `ISO 8601` format ||
|| **status** 
[`string`](../../../data-types.md) | Status of the sprint: `planned` — planned, `active` — active, `completed` — completed ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "Sprint not found"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | `Sprint id not found` | `0` is passed in the `id` parameter ||
|| `400` | `0` | `Access denied` | No access to the Scrum ||
|| `400` | `0` | `Sprint not found` | The sprint with the specified `id` is not found ||
|| `400` | `100` | `Could not find value for parameter {id}` | The `id` parameter is not passed ||
|| `400` | `100` | `Invalid value {stringValue} to match with parameter {id}. Should be value of type int` | The `id` parameter is not a number ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-sprint-add.md)
- [{#T}](./tasks-api-scrum-sprint-update.md)
- [{#T}](./tasks-api-scrum-sprint-list.md)
- [{#T}](./tasks-api-scrum-sprint-delete.md)
- [{#T}](./tasks-api-scrum-sprint-start.md)
- [{#T}](./tasks-api-scrum-sprint-complete.md)
- [{#T}](./tasks-api-scrum-sprint-get-fields.md)
