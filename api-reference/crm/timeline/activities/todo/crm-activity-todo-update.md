# Update Universal Activity crm.activity.todo.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM item for which the activity is being updated

The `crm.activity.todo.update` method updates several properties of a universal activity in a single call.

{% note warning "" %}

The method overwrites the activity: if you do not pass `title`, `description`, or `pingOffsets`, the title and description become empty, and the reminders are deleted. To change a single property, use the [crm.activity.todo.updateDeadline](./crm-activity-todo-update-deadline.md), [crm.activity.todo.updateDescription](./crm-activity-todo-update-description.md), [crm.activity.todo.updateColor](./crm-activity-todo-update-color.md), or [crm.activity.todo.updateResponsibleUser](./crm-activity-todo-update-responsible-user.md) methods.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Identifier of the activity being updated, for example `999`.

You can retrieve the identifier using the [crm.activity.list](../activity-base/crm-activity-list.md) method with the filter `PROVIDER_ID` = `CRM_TODO`. A closed activity cannot be updated ||
|| **ownerTypeId***
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) in whose timeline the activity is located, for example `2` for a deal ||
|| **ownerId***
[`integer`](../../../../data-types.md) | Identifier of the CRM entity in whose timeline the activity is located, for example `1`.

The `ownerTypeId` and `ownerId` values are returned by the [crm.activity.get](../activity-base/crm-activity-get.md) method in the `OWNER_TYPE_ID` and `OWNER_ID` fields. If the activity does not belong to this entity, the method returns the `NOT_FOUND` error ||
|| **deadline***
[`datetime`](../../../../data-types.md) | Deadline for the activity in ISO 8601 format, for example `2026-11-03T15:00:00+01:00`.

If you do not specify a time zone, the time is interpreted in the time zone of the user on whose behalf the request is executed. If you pass only a date, for example `2026-11-03`, the deadline is set to the start of the day, `00:00` ||
|| **title**
[`string`](../../../../data-types.md) | Title of the activity. If the parameter is not passed, the title becomes empty ||
|| **description**
[`string`](../../../../data-types.md) | Description of the activity. Supports BB code, for example `[b]important[/b]`. If the parameter is not passed, the description becomes empty ||
|| **responsibleId**
[`integer`](../../../../data-types.md) | Identifier of the employee responsible for the activity, for example `1`. You can retrieve the identifier using the [user.get](../../../../user/user-get.md) method.

Only a company employee can be assigned as responsible. If the parameter is not passed or equals `0`, the responsible user does not change ||
|| **parentActivityId**
[`integer`](../../../../data-types.md) | Identifier of another activity in the timeline to link the activity to, for example `888`. You can retrieve the identifier using the [crm.activity.list](../activity-base/crm-activity-list.md) method.

If the parameter is not passed or the activity is not found, the current link is retained ||
|| **pingOffsets**
[`array`](../../../../data-types.md) | Array of integers: how many minutes before the deadline to send reminders. For example, `[0, 15]` creates 2 reminders: 15 minutes before the deadline and at the moment it occurs.

The method discards negative and non-numeric values and merges duplicates. If the parameter is not passed, all reminders are deleted ||
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

If the parameter is not passed or the value is not in the list, the color does not change. The method does not return an error for an invalid value ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"ownerTypeId":2,"ownerId":1,"deadline":"2026-11-04T15:00:00+01:00","title":"New case title","description":"New case description","responsibleId":1,"pingOffsets":[15,30],"colorId":"7"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.todo.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"ownerTypeId":2,"ownerId":1,"deadline":"2026-11-04T15:00:00+01:00","title":"New case title","description":"New case description","responsibleId":1,"pingOffsets":[15,30],"colorId":"7","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.todo.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TodoUpdateResult = {
      id: number,
    }

    try {
      const response = await $b24.actions.v2.call.make<TodoUpdateResult>({
        method: 'crm.activity.todo.update',
        params: {
          id: 999,
          ownerTypeId: 2,
          ownerId: 1,
          deadline: '2026-11-04T15:00:00+01:00',
          title: 'New activity title',
          description: 'New activity description',
          responsibleId: 1,
          pingOffsets: [15, 30],
          colorId: '7',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated todo activity id:', result.id)
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
      async function updateTodoActivity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.todo.update',
            params: {
              id: 999,
              ownerTypeId: 2,
              ownerId: 1,
              deadline: '2026-11-04T15:00:00+01:00',
              title: 'New activity title',
              description: 'New activity description',
              responsibleId: 1,
              pingOffsets: [15, 30],
              colorId: '7',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Updated todo activity id:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateTodoActivity)
    </script>
    ```

- Python

    ```python

    from datetime import datetime, timedelta

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.todo.update(
            bitrix_id=999,
            owner_type_id=2,
            owner_id=1,
            deadline=datetime.now() + timedelta(days=2),
            title="Updated follow-up",
            description="Send updated proposal",
            responsible_id=1,
            ping_offsets=[15, 30],
            color_id="7",
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
                'crm.activity.todo.update',
                [
                    'id'            => 999,
                    'ownerTypeId'   => 2,
                    'ownerId'       => 1,
                    'deadline'      => (new DateTime('+1 day'))->format(DATE_ATOM),
                    'title'         => 'New case title',
                    'description'   => 'New case description',
                    'responsibleId' => 1,
                    'pingOffsets'   => [15, 30],
                    'colorId'       => '7'
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
        "crm.activity.todo.update",
        {
            id: 999,
            ownerTypeId: 2,
            ownerId: 1,
            deadline: new Date().toISOString(),
            title: 'New case title',
            description: 'New case description',
            responsibleId: 1,
            pingOffsets: [15, 30],
            colorId: '7'
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
        'crm.activity.todo.update',
        [
            'id' => 999,
            'ownerTypeId' => 2,
            'ownerId' => 1,
            'deadline' => date('c', strtotime('+1 day')), // Tomorrow at the same time in ISO 8601 format
            'title' => 'New case title',
            'description' => 'New case description',
            'responsibleId' => 1,
            'pingOffsets' => [15, 30],
            'colorId' => '7'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.todo.update", b24.Params{
    	"id":            999,
    	"ownerTypeId":   2,
    	"ownerId":       1,
    	"deadline":      time.Now().Add(24 * time.Hour).Format(time.RFC3339),
    	"title":         "New case title",
    	"description":   "New case description",
    	"responsibleId": 1,
    	"pingOffsets":   []int{15, 30},
    	"colorId":       "7",
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.todo.update: %w", err)
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

HTTP status: **200**

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
[`object`](../../../../data-types.md) | Object with the identifier of the updated activity [(detailed description)](#result) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the updated activity, matches the passed `id` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Item was not found."
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {deadline} | The required parameter `ownerTypeId`, `ownerId`, or `deadline` is not passed. The description contains the parameter name ||
|| `400` | `0` | Internal server error. | The required parameter `id` is not passed ||
|| `400` | `100` | Invalid value {3} to match with parameter {colorId}. Should be value of type string. | The `colorId` value is passed as a number, not a string ||
|| `400` | `WRONG_DATETIME_FORMAT` | Incorrect date format | The `deadline` value could not be recognized as a date ||
|| `400` | `NOT_FOUND` | Item was not found | No activity with this `id` exists, or it does not belong to the CRM entity from `ownerTypeId` and `ownerId` ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have permission to edit the CRM entity ||
|| `400` | `0` | Cannot update files in a completed activity | The activity is closed and cannot be updated ||
|| `400` | `0` | The specified user cannot be assigned as responsible | The user from `responsibleId` is not found or is not a company employee ||
|#

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-todo-add.md)
- [{#T}](./crm-activity-todo-update-deadline.md)
- [{#T}](./crm-activity-todo-update-description.md)
- [{#T}](./crm-activity-todo-update-color.md)
- [{#T}](./crm-activity-todo-update-responsible-user.md)
- [{#T}](../activity-base/crm-activity-get.md)
- [{#T}](../activity-base/crm-activity-list.md)
- [{#T}](../activity-base/crm-activity-delete.md)
- [{#T}](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-date-in-activity.md)
