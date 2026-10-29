# Add Universal Activity crm.activity.todo.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM entity to whose timeline the activity is being added

The method `crm.activity.todo.add` adds a universal activity to the timeline of a CRM entity.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerTypeId***
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) to whose timeline the activity is added, for example `2` for a deal. If the type does not exist, the method returns the `NOT_FOUND` error ||
|| **ownerId***
[`integer`](../../../../data-types.md) | Identifier of the CRM entity to whose timeline the activity is added, for example `1`.

You can retrieve the identifier using the [crm.item.list](../../../universal/crm-item-list.md) method ||
|| **deadline***
[`datetime`](../../../../data-types.md) | Deadline for the activity in ISO 8601 format, for example `2026-11-03T15:00:00+01:00`.

If you do not specify a time zone, the time is interpreted in the time zone of the user on whose behalf the request is executed. If you pass only a date, for example `2026-11-03`, the deadline is set to the start of the day, `00:00` ||
|| **title**
[`string`](../../../../data-types.md) | Title of the activity. Default is an empty string ||
|| **description**
[`string`](../../../../data-types.md) | Description of the activity. Supports BB code, for example `[b]important[/b]`. Default is an empty string ||
|| **responsibleId**
[`integer`](../../../../data-types.md) | Identifier of the employee responsible for the activity, for example `1`. You can retrieve the identifier using the [user.get](../../../../user/user-get.md) method.

Only a company employee can be assigned as responsible. If the user is not found or is an external user, the method returns the `0` error with the description `The specified user cannot be assigned as responsible`.

By default, and also when the value is `0`, the user on whose behalf the request is executed becomes responsible. If the responsible user differs from this user, they receive a notification about the new activity ||
|| **parentActivityId**
[`integer`](../../../../data-types.md) | Identifier of the previous activity in the timeline, for example `888`. You can retrieve the identifier using the [crm.activity.list](../activity-base/crm-activity-list.md) method.

The new activity is linked to the previous one, and the previous activity is closed. If no activity with this identifier is found, the parameter is ignored ||
|| **pingOffsets**
[`array`](../../../../data-types.md) | Array of integers: how many minutes before the deadline to send reminders. For example, `[0, 15]` creates 2 reminders: 15 minutes before the deadline and at the moment it occurs.

The method discards negative and non-numeric values and merges duplicates. Default is an empty array, with no reminders ||
|| **colorId**
[`string`](../../../../data-types.md) | Identifier of the activity color in the timeline. Pass the value as a string, for example `"3"`: a number causes the `100` error. Possible values:

- `default` — yellow, the default color
- `1` — blue
- `2` — turquoise
- `3` — orange
- `4` — green
- `5` — purple
- `6` — gray
- `7` — pink

If the parameter is not passed or the value is not in the list, the activity gets the default color. The method does not return an error for an invalid value ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerTypeId":2,"ownerId":1,"deadline":"'"$(date -Iseconds)"'","title":"Activity Title","description":"Activity Description","responsibleId":5,"pingOffsets":[0,15],"colorId":"2"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.todo.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerTypeId":2,"ownerId":1,"deadline":"'"$(date -Iseconds)"'","title":"Activity Title","description":"Activity Description","responsibleId":5,"pingOffsets":[0,15],"colorId":"2","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.todo.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TodoAddResult = {
      id: number
    }

    try {
      const response = await $b24.actions.v2.call.make<TodoAddResult>({
        method: 'crm.activity.todo.add',
        params: {
          ownerTypeId: 2,
          ownerId: 1,
          deadline: new Date().toISOString(),
          title: 'Todo activity title',
          description: 'Todo activity description',
          responsibleId: 5,
          pingOffsets: [0, 15],
          colorId: '2',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added todo activity ID:', result.id)
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
      async function addTodoActivity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.todo.add',
            params: {
              ownerTypeId: 2,
              ownerId: 1,
              deadline: new Date().toISOString(),
              title: 'Todo activity title',
              description: 'Todo activity description',
              responsibleId: 5,
              pingOffsets: [0, 15],
              colorId: '2',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Added todo activity ID:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTodoActivity)
    </script>
    ```

- Python

    ```python

    from datetime import datetime, timedelta

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.todo.add(
            owner_type_id=2,
            owner_id=1,
            deadline=datetime.now() + timedelta(days=1),
            title="Follow up with customer",
            description="Discuss proposal details",
            responsible_id=5,
            ping_offsets=[0, 15],
            color_id="2",
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
                'crm.activity.todo.add',
                [
                    'ownerTypeId'   => 2,
                    'ownerId'       => 1,
                    'deadline'      => (new DateTime())->format(DATE_ATOM),
                    'title'         => 'Activity Title',
                    'description'   => 'Activity Description',
                    'responsibleId' => 5,
                    'pingOffsets'   => [0, 15],
                    'colorId'       => '2'
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
        echo 'Error adding todo activity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.activity.todo.add",
        {
            ownerTypeId: 2,
            ownerId: 1,
            deadline: new Date().toISOString(),
            title: 'Activity Title',
            description: 'Activity Description',
            responsibleId: 5,
            pingOffsets: [0, 15],
            colorId: '2'
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.todo.add',
        [
            'ownerTypeId' => 2,
            'ownerId' => 1,
            'deadline' => date('c'), // Current date and time in ISO 8601 format
            'title' => 'Activity Title',
            'description' => 'Activity Description',
            'responsibleId' => 5,
            'pingOffsets' => [0, 15],
            'colorId' => '2'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.todo.add", b24.Params{
    	"ownerTypeId":   2,
    	"ownerId":       1,
    	"deadline":      time.Now().Format(time.RFC3339),
    	"title":         "Activity Title",
    	"description":   "Activity Description",
    	"responsibleId": 5,
    	"pingOffsets":   []int{0, 15},
    	"colorId":       "2",
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.todo.add: %w", err)
    }

    var item struct {
    	ID b24.ID `json:"id"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "id": 999
    },
    "time": {
       "start": 1724068028.331234,
        "finish": 1724068028.726591,
        "duration": 0.3953571319580078,
        "processing": 0.13033390045166016,
        "date_start": "2025-01-21T13:47:08+02:00",
        "date_finish": "2025-01-21T13:47:08+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Object with the identifier of the added activity [(detailed description)](#result) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the added activity. Use it in the `id` parameter of the [crm.activity.todo.update](./crm-activity-todo-update.md), [crm.activity.get](../activity-base/crm-activity-get.md), and [crm.activity.delete](../activity-base/crm-activity-delete.md) methods ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "WRONG_DATETIME_FORMAT",
    "error_description": "Incorrect date format."
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {deadline} | The required parameter `ownerTypeId`, `ownerId`, or `deadline` is not passed. The description contains the parameter name ||
|| `400` | `100` | Invalid value {3} to match with parameter {colorId}. Should be value of type string. | The `colorId` value is passed as a number, not a string ||
|| `400` | `WRONG_DATETIME_FORMAT` | Incorrect date format | The `deadline` value could not be recognized as a date ||
|| `400` | `NOT_FOUND` | Item was not found | A non-existent CRM object type is passed in `ownerTypeId` ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have permission to edit the CRM entity ||
|| `400` | `0` | The specified user cannot be assigned as responsible | The user from `responsibleId` is not found or is not a company employee ||
|| `400` | `CAN_NOT_COMPLETE` | Activity completion error text | The activity is created, but the previous activity from `parentActivityId` could not be closed ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-todo-update.md)
- [{#T}](./crm-activity-todo-update-deadline.md)
- [{#T}](./crm-activity-todo-update-description.md)
- [{#T}](./crm-activity-todo-update-color.md)
- [{#T}](./crm-activity-todo-update-responsible-user.md)
- [{#T}](../activity-base/crm-activity-get.md)
- [{#T}](../activity-base/crm-activity-list.md)
- [{#T}](../activity-base/crm-activity-delete.md)
- [{#T}](../../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-objects-with-crm-mode.md)
