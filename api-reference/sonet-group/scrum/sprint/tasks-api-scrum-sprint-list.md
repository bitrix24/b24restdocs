# Get a List of Sprints tasks.api.scrum.sprint.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.api.scrum.sprint.list` returns a list of sprints.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

All parameters are optional. The method returns only sprints of Scrums the user is a member of; without parameters, it returns all such sprints. Specify field names in `order`, `filter`, and `select` in uppercase. Available fields are listed in the [Available Fields for filter, order, and select](#fields) table.

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`object`](../../../data-types.md) | An object of type `{'sort_field': 'sort_direction' [, ...]}`, for example, `{"ID": "desc"}`.

The sort direction can take the following values:
- `asc` — ascending
- `desc` — descending ||
|| **filter**
[`object`](../../../data-types.md) | An object of type `{'filterable_field': 'filter_value' [, ...]}`, for example, `{"GROUP_ID": 1, "STATUS": "active"}`.

You can put an operator before the field name:
- `>` and `<` — greater than and less than
- `>=` and `<=` — greater than or equal to, less than or equal to
- `!` — not equal to
- `%` — contains a substring

For example, `{">ID": 20}` or `{"%NAME": "Sprint"}`.

If you specify a nonexistent field, for example, `groupId` instead of `GROUP_ID`, the method returns an empty array without an error ||
|| **select**
[`array`](../../../data-types.md) | An array of fields to fill in the response, for example, `["ID", "NAME", "STATUS"]`.

If the array contains the value `"*"` or the array is not passed, all fields are filled.

The response always contains the full set of sprint keys. Fields not listed in `select` are returned with empty values: `0` for numbers and `""` for strings ||
|| **start**
[`integer`](../../../data-types.md) | Offset for pagination. The method returns up to 50 sprints per call. To retrieve the next page, increase `start` by 50. The response does not contain the `total` and `next` fields: if fewer than 50 sprints are returned, this is the last page ||
|#

### Available Fields for filter, order, and select {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID** 
[`integer`](../../../data-types.md) | Identifier of the sprint ||
|| **GROUP_ID** 
[`integer`](../../../data-types.md) | Scrum identifier. You can obtain the identifier using the [socialnetwork.api.workgroup.list](../../socialnetwork-api-workgroup-list.md) method ||
|| **ENTITY_TYPE** 
[`string`](../../../data-types.md) | Item type, always `sprint` for sprints ||
|| **NAME** 
[`string`](../../../data-types.md) | Name of the sprint ||
|| **SORT** 
[`integer`](../../../data-types.md) | Sort order of the sprint ||
|| **CREATED_BY** 
[`integer`](../../../data-types.md) | Identifier of the user who created the sprint ||
|| **MODIFIED_BY** 
[`integer`](../../../data-types.md) | Identifier of the user who modified the sprint ||
|| **DATE_START** 
[`string`](../../../data-types.md) | Start date of the sprint. You can use the field in `order` and `select`. Filtering by date does not work: the method returns an empty array for any value format ||
|| **DATE_END** 
[`string`](../../../data-types.md) | End date of the sprint. You can use the field in `order` and `select`. Filtering by date does not work, as with `DATE_START` ||
|| **STATUS** 
[`string`](../../../data-types.md) | Status: `planned` — planned, `active` — active, `completed` — completed ||
|| **INFO** 
[`object`](../../../data-types.md) | Service field. Not returned in the method response ||
|#

There is no `GOAL` field in `filter`, `order`, and `select`: with it, the method returns an empty array. The sprint goal is returned only in the response, in the `goal` field.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "filter": {
        "GROUP_ID": 1,
        "STATUS": "active"
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "filter": {
        "GROUP_ID": 1,
        "STATUS": "active"
    },
    "auth": "YOUR_ACCESS_TOKEN"
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each sprint returned in result[]
    type Sprint = {
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

    const groupId = 1

    // tasks.api.scrum.sprint.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    try {
      const response = await $b24.actions.v2.call.make<Sprint[]>({
        method: 'tasks.api.scrum.sprint.list',
        params: {
          filter: {
            GROUP_ID: groupId,
            STATUS: 'active',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Sprints:', result.length, result)
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
      async function listScrumSprints() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const groupId = 1

          // tasks.api.scrum.sprint.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'tasks.api.scrum.sprint.list',
            params: {
              filter: {
                GROUP_ID: groupId,
                STATUS: 'active',
              },
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
          console.info('Sprints:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listScrumSprints)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.api.scrum.sprint.list(
            filter={
                "GROUP_ID": 1,
                "STATUS": "active",
            },
            start=0,
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
    $groupId = 1;
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.sprint.list',
                [
                    'filter' => [
                        'GROUP_ID'    => $groupId,
                        'STATUS'   => 'active',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching sprint list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const groupId = 1;
    BX24.callMethod(
        'tasks.api.scrum.sprint.list',
        {
            filter: {
                GROUP_ID: groupId,
                STATUS: 'active'
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
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.list',
        [
            'filter' => [
                'GROUP_ID' => 1,
                'STATUS' => 'active'
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
    res, err := client.Core().Call(ctx, "tasks.api.scrum.sprint.list", b24.Params{
    	"filter": b24.Params{
    		"GROUP_ID": 1,
    		"STATUS":   "active",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.api.scrum.sprint.list: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "id": 3,
            "groupId": 1,
            "entityType": "sprint",
            "name": "Sprint 1",
            "goal": "",
            "sort": 1,
            "createdBy": 1,
            "modifiedBy": 1,
            "dateStart": "2021-11-21T22:00:00+00:00",
            "dateEnd": "2021-11-28T22:00:00+00:00",
            "status": "active"
        }
    ],
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
[`array`](../../../data-types.md) | Array of sprints. If no sprint matches the filter, the method returns an empty array, not an error. Array element fields [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### result Array Element {#result}

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
    "error_description": "Could not load list"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | `Could not load list` | An error occurred while executing the database query ||
|| `400` | `0` | PHP system error text, for example, `Cannot access offset of type string on string` | The `filter`, `order`, or `select` parameter is passed as a string instead of an object or array ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-sprint-add.md)
- [{#T}](./tasks-api-scrum-sprint-update.md)
- [{#T}](./tasks-api-scrum-sprint-get.md)
- [{#T}](./tasks-api-scrum-sprint-delete.md)
- [{#T}](./tasks-api-scrum-sprint-start.md)
- [{#T}](./tasks-api-scrum-sprint-complete.md)
- [{#T}](./tasks-api-scrum-sprint-get-fields.md)
