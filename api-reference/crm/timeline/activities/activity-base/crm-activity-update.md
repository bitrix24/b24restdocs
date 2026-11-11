# Update System CRM Activity crm.activity.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user with permission to update the activity

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [crm.activity.todo.update](../todo/crm-activity-todo-update.md).

{% endnote %}

The method `crm.activity.update` updates an existing activity.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Integer identifier of the activity in the timeline, for example `999`. Use [crm.activity.list](./crm-activity-list.md) to retrieve the identifier ||
|| **fields***
[`object`](../../../../data-types.md) | Object containing the activity fields to update [(detailed description)](#fields). Example structure:

```json
{
    "fields": {
        "SUBJECT": "New Call"
    }
}
```
||
|| **params**
[`object`](../../../../data-types.md) | Optional update parameters. The `REGISTER_HISTORY_EVENT` field accepts `Y` to register an event in the history or `N` to skip it. By default, the event is registered ||
|#

### Parameter fields {#fields}

Pass only the fields you need to change in `fields`. The full list of fields and their descriptions is available in [crm.activity.fields](./crm-activity-fields.md). A field marked as required in the metadata does not need to be passed with every update.

Field behavior during updates:

#|
|| **Field** `type` | **Description** ||
|| **OWNER_ID**
[`integer`](../../../../data-types.md) | Identifier of the current CRM item. The value passed during an update is ignored ||
|| **OWNER_TYPE_ID**
[`crm_enum_ownertype`](../../../data-types.md#object_type) | [Identifier of the current CRM object type](../../../data-types.md#object_type). The value passed during an update is ignored ||
|| **TYPE_ID**
[`crm_enum_activitytype`](../../../data-types.md#activity-enums) | The activity type cannot be changed. The value passed is ignored ||
|| **ASSOCIATED_ENTITY_ID**
[`integer`](../../../../data-types.md) | Read-only. The value passed is ignored ||
|| **COMMUNICATIONS**
[`crm_activity_communication[]`](../../../data-types.md) | Array of [communications](./crm-activity-communication-fields.md). If provided, it must contain a valid communication. An empty array or a list without valid communications causes an error ||
|| **COMPLETED**
[`char`](../../../../data-types.md) | Activity completion flag: `Y` — completed, `N` — not completed ||
|| **DEADLINE**
[`datetime`](../../../../data-types.md) | Cannot be changed directly: the value passed is ignored ||
|| **FILES**
[`diskfile[]`](../../../data-types.md) | Array of files. For each file, pass an object with `FILE_ID`, the file identifier in Drive ||
|| **PROVIDER_ID**
[`string`](../../../../data-types.md) | The activity provider cannot be changed. The value passed is ignored ||
|| **RESPONSIBLE_ID**
[`user`](../../../../data-types.md) | Identifier of the responsible user. Not required for every update ||
|| **SUBJECT**
[`string`](../../../../data-types.md) | Activity subject. Not required for every update ||
|| **IS_INCOMING_CHANNEL**
[`char`](../../../../data-types.md) | Read-only. The value passed is ignored ||
|#

## Code Examples

{% include [Note on Examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"fields":{"RESPONSIBLE_ID":1,"DESCRIPTION":"New description of the activity"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"fields":{"RESPONSIBLE_ID":1,"DESCRIPTION":"New description of the activity"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'crm.activity.update',
        params: {
          id: 999,
          fields: {
            RESPONSIBLE_ID: 1,
            DESCRIPTION: 'New activity description',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Activity updated successfully:', result)
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
      async function updateActivity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.update',
            params: {
              id: 999,
              fields: {
                RESPONSIBLE_ID: 1,
                DESCRIPTION: 'New activity description',
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
          console.info('Activity updated successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateActivity)
    </script>
    ```

- Python

    ```python

    from datetime import datetime, timedelta

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.update(
            bitrix_id=999,
            fields={
                "SUBJECT": "Updated follow-up",
                "START_TIME": (datetime.now() + timedelta(days=1)).isoformat(timespec="seconds"),
                "DESCRIPTION": "Updated activity description",
                "RESPONSIBLE_ID": 1,
                "COMPLETED": "N",
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
                'crm.activity.update',
                [
                    'id' => 999,
                    'fields' => [
                        "RESPONSIBLE_ID" => 1,
                        "DESCRIPTION" => "New description of the activity"
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
        echo 'Error updating activity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.update',
        {
            id: 999,
            fields: {
                "RESPONSIBLE_ID": 1, 
                "DESCRIPTION": "New description of the activity"
            }
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.update',
        [
            'id' => 999,
            'fields' => [
                'RESPONSIBLE_ID' => 1,
                'DESCRIPTION' => 'New description of the activity'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.update", b24.Params{
    	"id": 999,
    	"fields": b24.Params{
    		"RESPONSIBLE_ID": 1,
    		"DESCRIPTION":    "New description of the activity",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.update: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | Returns `true` if the activity was successfully updated. On failure, the method returns an error response; the error description may be empty ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

For example, when `id = 0`, the method returns an error:

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "ID is not defined or invalid."
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Meaning** ||
|| `400` | Empty value | `ID is not defined or invalid.` | The activity identifier is missing or invalid ||
|| `400` | Empty value | `Parameter 'fields' must be array.` | The `fields` parameter was not passed as an object ||
|| `400` | Empty value | `Parameter 'params' must be array.` | The `params` parameter was not passed as an object ||
|| `400` | Empty value | `Activity is not found.` | The activity with the specified identifier was not found ||
|| `400` | Empty value | `The field COMMUNICATIONS is not defined or invalid.` | An empty `COMMUNICATIONS` array or a list without valid communications was passed ||
|| `400` | Empty value | `Use crm.activity.configurable.update for this activity provider` | Use `crm.activity.configurable.update` to update a configurable activity ||
|| `400` | Empty value | `Access denied.` | No permission to update the current CRM item; for an app activity, the call was made by another app ||
|| `400` | Empty value | `Application context required.` | Updating an activity with the `REST_APP` provider requires the context of the app that created it ||
|#

When accessing a nonexistent activity, the response may also contain empty `error` and `error_description` values with HTTP status `400`.

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-activity-add.md)
- [{#T}](./crm-activity-get.md)
- [{#T}](./crm-activity-list.md)
- [{#T}](./crm-activity-delete.md)
- [{#T}](./crm-activity-communication-fields.md)
- [{#T}](./crm-activity-fields.md)