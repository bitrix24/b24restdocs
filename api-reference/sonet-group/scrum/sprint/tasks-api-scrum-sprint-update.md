# Update Sprint tasks.api.scrum.sprint.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

The method `tasks.api.scrum.sprint.update` updates a sprint.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the sprint.

You can obtain the identifier using the [tasks.api.scrum.sprint.list](./tasks-api-scrum-sprint-list.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Object containing sprint data [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **groupId** 
[`integer`](../../../data-types.md) | Identifier of the group (Scrum) to which the sprint belongs. 

You can obtain the identifier using the [socialnetwork.api.workgroup.list](../../socialnetwork-api-workgroup-list.md) method. A group is a Scrum if its `SCRUM_MASTER_ID` field is filled.

A different `groupId` moves the sprint to another Scrum. Only a sprint without tasks can be moved ||
|| **name** 
[`string`](../../../data-types.md) | Name of the sprint ||
|| **createdBy** 
[`integer`](../../../data-types.md) | Identifier of the user who created the sprint.

You can obtain the identifier using the [user.get](../../../user/user-get.md) method ||
|| **modifiedBy** 
[`integer`](../../../data-types.md) | Identifier of the user who modified the sprint ||
|| **sort** 
[`integer`](../../../data-types.md) | Sort order of the sprint ||
|| **dateStart** 
[`string`](../../../data-types.md) | Start date of the sprint. Available formats: `ISO 8601`, `timestamp` ||
|| **dateEnd** 
[`string`](../../../data-types.md) | End date of the sprint. Available formats: `ISO 8601`, `timestamp` ||
|| **status** 
[`string`](../../../data-types.md) | Status of the sprint. Available values:
- `planned` — planned
- `active` — active. A Scrum can have only one active sprint
- `completed` — completed

To start or complete a sprint, use the [tasks.api.scrum.sprint.start](./tasks-api-scrum-sprint-start.md) and [tasks.api.scrum.sprint.complete](./tasks-api-scrum-sprint-complete.md) methods ||
|#

Pass in `fields` only the fields you want to change. The other sprint fields keep their current values.

{% note warning "" %}

The method does not validate the date format when updating. If you pass a string that is not a date in `dateStart` or `dateEnd`, the method returns a successful response and writes the date `1970-01-01`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 2,
    "fields": {
        "name": "Sprint 2",
        "groupId": 1,
        "dateStart": "2021-11-22T00:00:00+02:00",
        "dateEnd": "2021-11-29T00:00:00+02:00"
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "id": 2,
    "fields": {
        "name": "Sprint 2",
        "groupId": 1,
        "dateStart": "2021-11-22T00:00:00+02:00",
        "dateEnd": "2021-11-29T00:00:00+02:00"
    }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SprintResult = {
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
      const response = await $b24.actions.v2.call.make<SprintResult>({
        method: 'tasks.api.scrum.sprint.update',
        params: {
          id: 2,
          fields: {
            name: 'Sprint 2',
            groupId: 1,
            dateStart: '2021-11-22T00:00:00+02:00',
            dateEnd: '2021-11-29T00:00:00+02:00',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated sprint:', result.id, result.name, result.status)
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
      async function updateSprint() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.api.scrum.sprint.update',
            params: {
              id: 2,
              fields: {
                name: 'Sprint 2',
                groupId: 1,
                dateStart: '2021-11-22T00:00:00+02:00',
                dateEnd: '2021-11-29T00:00:00+02:00',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Updated sprint:', result.id, result.name, result.status)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateSprint)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.api.scrum.sprint.update(
            bitrix_id=2,
            fields={
                "name": "Sprint 2",
                "groupId": 1,
                "dateStart": "2021-11-22T00:00:00+02:00",
                "dateEnd": "2021-11-29T00:00:00+02:00",
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
    $sprintId = 2;
    $groupId = 1;
    $name = 'Sprint 2';
    $dateStart = '2021-11-22T00:00:00+02:00';
    $dateEnd = '2021-11-29T00:00:00+02:00';

    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.sprint.update',
                [
                    'id' => $sprintId,
                    'fields' => [
                        'name'      => $name,
                        'groupId'   => $groupId,
                        'dateStart' => $dateStart,
                        'dateEnd'   => $dateEnd,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating sprint: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const sprintId = 2;
    const groupId = 1;
    const name = 'Sprint 2';
    const dateStart = '2021-11-22T00:00:00+02:00';
    const dateEnd = '2021-11-29T00:00:00+02:00';
    BX24.callMethod(
        'tasks.api.scrum.sprint.update',
        {
            id: sprintId,
            fields: {
                name: name,
                groupId: groupId,
                dateStart: dateStart,
                dateEnd: dateEnd,
            }
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

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.update',
        [
            'id' => 2,
            'fields' => [
                'name' => 'Sprint 2',
                'groupId' => 1,
                'dateStart' => '2021-11-22T00:00:00+02:00',
                'dateEnd' => '2021-11-29T00:00:00+02:00'
            ]
        ]
    );

    // Processing the response from Bitrix24
    if (isset($result['error'])) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.api.scrum.sprint.update", b24.Params{
    	"id": 2,
    	"fields": b24.Params{
    		"name":      "Sprint 2",
    		"groupId":   1,
    		"dateStart": "2021-11-22T00:00:00+02:00",
    		"dateEnd":   "2021-11-29T00:00:00+02:00",
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.api.scrum.sprint.update: %w", err)
    }

    var item struct {
    	ID         b24.ID `json:"id"`
    	GroupID    b24.ID `json:"groupId"`
    	EntityType string `json:"entityType"`
    	Name       string `json:"name"`
    	Goal       string `json:"goal"`
    	Sort       int    `json:"sort"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.GroupID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 2,
        "groupId": 1,
        "entityType": "sprint",
        "name": "Sprint 2",
        "goal": "",
        "sort": 1,
        "createdBy": 1,
        "modifiedBy": 1,
        "dateStart": "2021-11-22T00:00:00+02:00",
        "dateEnd": "2021-11-29T00:00:00+02:00",
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
|| `400` | `0` | `Access denied` | No access to the Scrum ||
|| `400` | `0` | `Sprint id not found` | `0` is passed in the `id` parameter ||
|| `400` | `0` | `Sprint not found` | The sprint with the specified `id` is not found ||
|| `400` | `0` | `It is forbidden move a sprint with items` | A different `groupId` is passed, but the sprint contains tasks ||
|| `400` | `0` | `createdBy user not found` | The user with the identifier from the `createdBy` field is not found ||
|| `400` | `0` | `modifiedBy user not found` | The user with the identifier from the `modifiedBy` field is not found ||
|| `400` | `0` | `Unable to add two active sprint` | The sprint cannot be switched to the `active` status: the Scrum already has an active sprint ||
|| `400` | `0` | `Incorrect sprint status` | The `status` value is not one of `planned`, `active`, `completed` ||
|| `400` | `0` | `Sprint not updated` | Failed to save the sprint changes ||
|| `400` | `100` | `Could not find value for parameter {id}` | The `id` parameter is not passed ||
|| `400` | `100` | `Invalid value {stringValue} to match with parameter {id}. Should be value of type int` | The `id` parameter is not a number ||
|| `400` | `100` | `Could not find value for parameter {fields}` | The `fields` parameter is not passed ||
|| `400` | `100` | `Invalid value {stringValue} to match with parameter {fields}. Should be value of type array` | The `fields` parameter is not an object ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-sprint-add.md)
- [{#T}](./tasks-api-scrum-sprint-get.md)
- [{#T}](./tasks-api-scrum-sprint-list.md)
- [{#T}](./tasks-api-scrum-sprint-delete.md)
- [{#T}](./tasks-api-scrum-sprint-start.md)
- [{#T}](./tasks-api-scrum-sprint-complete.md)
- [{#T}](./tasks-api-scrum-sprint-get-fields.md)
