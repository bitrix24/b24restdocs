# Update Deadline for Universal Activity crm.activity.todo.updateDeadline

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with edit access permission for the CRM object associated with the activity being updated.

The method `crm.activity.todo.updateDeadline` changes the deadline of a universal activity.

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
|| **value***
[`datetime`](../../../../data-types.md) | New deadline for the activity in ISO 8601 format, for example `2026-11-03T15:00:00+01:00`.

If you do not specify a time zone, the time is interpreted in the time zone of the user on whose behalf the request is executed. If you pass only a date, for example `2026-11-03`, the deadline is set to the start of the day, `00:00`.

If the activity is linked to a calendar event, the event is moved to the new deadline. This requires permission to edit the event ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"ownerTypeId":2,"ownerId":1,"value":"'"$(date -Iseconds)"'"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.todo.updateDeadline
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"ownerTypeId":2,"ownerId":1,"value":"2026-11-05T12:30:00+01:00","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.todo.updateDeadline
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UpdateDeadlineResult = {
      id: number
    }

    try {
      const response = await $b24.actions.v2.call.make<UpdateDeadlineResult>({
        method: 'crm.activity.todo.updateDeadline',
        params: {
          id: 999,
          ownerTypeId: 2,
          ownerId: 1,
          value: new Date().toISOString(),
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated activity id:', result.id)
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
      async function updateTodoDeadline() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.todo.updateDeadline',
            params: {
              id: 999,
              ownerTypeId: 2,
              ownerId: 1,
              value: new Date().toISOString(),
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Updated activity id:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateTodoDeadline)
    </script>
    ```

- Python

    ```python

    from datetime import datetime, timedelta

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.todo.update_deadline(
            bitrix_id=999,
            owner_type_id=2,
            owner_id=1,
            value=datetime.now() + timedelta(days=3),
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
                'crm.activity.todo.updateDeadline',
                [
                    'id'          => 999,
                    'ownerTypeId' => 2,
                    'ownerId'     => 1,
                    'value'       => date(DATE_ATOM),
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
        "crm.activity.todo.updateDeadline",
        {
            id: 999,
            ownerTypeId: 2,
            ownerId: 1,
            value: new Date().toISOString()
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
        'crm.activity.todo.updateDeadline',
        [
            'id' => 999,
            'ownerTypeId' => 2,
            'ownerId' => 1,
            'value' => date('c') // Current date and time in ISO 8601 format
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.todo.updateDeadline", b24.Params{
    	"id":          999,
    	"ownerTypeId": 2,
    	"ownerId":     1,
    	"value":       time.Now().Format(time.RFC3339),
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.todo.updateDeadline: %w", err)
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
[`object`](../../../../data-types.md) | Object with the identifier of the updated activity [(detailed description)](#result) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the updated activity, matches the passed `id` ||
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
|| `400` | `100` | Could not find value for parameter {value} | The required parameter `id`, `ownerTypeId`, `ownerId`, or `value` is not passed. The description contains the parameter name ||
|| `400` | `NOT_FOUND` | Item was not found | No activity with this `id` exists, or it does not belong to the CRM entity from `ownerTypeId` and `ownerId` ||
|| `400` | `WRONG_DATETIME_FORMAT` | Incorrect date format | The `value` could not be recognized as a date ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have permission to edit the CRM entity ||
|| `400` | `ACCESS_DENIED` | Insufficient permissions to change activity calendar event | The activity is linked to a calendar event, and the user does not have permission to edit this event ||
|| `400` | `CAN_NOT_UPDATE_COMPLETED_TODO` | Cannot edit completed activity | The activity is closed and cannot be updated ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-todo-add.md)
- [{#T}](./crm-activity-todo-update.md)
- [{#T}](./crm-activity-todo-update-description.md)
- [{#T}](./crm-activity-todo-update-color.md)
- [{#T}](./crm-activity-todo-update-responsible-user.md)
- [{#T}](../activity-base/crm-activity-get.md)
- [{#T}](../activity-base/crm-activity-list.md)
- [{#T}](../activity-base/crm-activity-delete.md)