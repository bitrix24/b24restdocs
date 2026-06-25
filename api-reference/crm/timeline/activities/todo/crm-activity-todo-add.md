# Add Universal Activity crm.activity.todo.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM object to which the activity is being added

The method `crm.activity.todo.add` adds a universal activity to the timeline.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerTypeId***
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) to which the activity is linked, for example `2` for a deal ||
|| **ownerId***
[`integer`](../../../../data-types.md) | Identifier of the CRM object to which the activity is linked, for example, `1` ||
|| **deadline***
[`datetime`](../../../../data-types.md) | Deadline for the activity, for example `2025-02-03T15:00:00` ||
|| **title**
[`string`](../../../../data-types.md) | Title of the activity, default is an empty string ||
|| **description**
[`string`](../../../../data-types.md) | Description of the activity, default is an empty string ||
|| **responsibleId**
[`integer`](../../../../data-types.md) | Identifier of the user responsible for the activity, for example `1` ||
|| **parentActivityId**
[`integer`](../../../../data-types.md) | Identifier of the activity in the timeline with which the created activity can be linked, for example `888` ||
|| **pingOffsets**
[`array`](../../../../data-types.md) | An array containing integer values in minutes that allow you to set reminder times for the activity. For example, `[0, 15]` means that 2 reminders will be created, one 15 minutes before the deadline and one at the moment the deadline occurs. Default is an empty array, with no reminders ||
|| **colorId**
[`string`](../../../../data-types.md) | Identifier of the activity color in the timeline, for example `1`. There are 8 colors available, values from 1 to 7 and a default color if none is specified:


||
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
                    'deadline'      => (new DateTime()),
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
            deadline: (new Date()),
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

- Python

    Example

    ```python
    from datetime import datetime, timedelta

    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.activity.todo.add(
            owner_type_id=2,
            owner_id=101,
            deadline=datetime.now() + timedelta(days=1),
            title="Follow up with customer",
            description="Discuss proposal details",
            responsible_id=1,
            parent_activity_id=998,
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
[`object`](../../../../data-types.md) | On success, returns an object containing the identifier of the added activity `id`, on error = `null` ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `100` | Required fields are missing ||
|| `NOT_FOUND` | CRM object not found ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|| `OWNER_NOT_FOUND` | Owner of the entity not found ||
|| `WRONG_DATETIME_FORMAT` | Incorrect date format ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-todo-update.md)
- [{#T}](./crm-activity-todo-update-deadline.md)
- [{#T}](./crm-activity-todo-update-description.md)
- [{#T}](./crm-activity-todo-update-color.md)
- [{#T}](./crm-activity-todo-update-responsible-user.md)
- [{#T}](../../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-objects-with-crm-mode.md)
