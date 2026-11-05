# Update Activity Link to CRM Item crm.activity.binding.move

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit both CRM items, the source and the target

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.activity.binding.move](../../../rest-v3/activities/crm-activity-binding-move-rest-v3.md).

{% endnote %}

The `crm.activity.binding.move` method moves an activity binding from one CRM item to another item of the same type. The activity disappears from the timeline of the source item and appears in the timeline of the target item.

For example, you can move an activity from one deal to another. You cannot move an activity from a deal to a contact with this method. To move an activity to a contact, bind it to the contact with the [crm.activity.binding.add](./crm-activity-binding-add.md) method and delete the binding to the deal with the [crm.activity.binding.delete](./crm-activity-binding-delete.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **activityId***
[`integer`](../../../../data-types.md) | Identifier of the activity in the timeline, for example `999`.

The ID can be retrieved with the [crm.activity.list](../activity-base/crm-activity-list.md) method ||
|| **sourceEntityTypeId***
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) to which the deal is linked, for example, `2` for a deal ||
|| **sourceEntityId***
[`integer`](../../../../data-types.md) | The identifier of the CRM item to which the activity is bound, for example, `1`.

The ID can be retrieved with the [crm.activity.binding.list](./crm-activity-binding-list.md) method ||
|| **targetEntityTypeId***
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) to which the activity should be bound, for example, `2` for a deal. Must match `sourceEntityTypeId` ||
|| **targetEntityId***
[`integer`](../../../../data-types.md) | The identifier of the CRM item to which the activity should be bound, for example, `100`.

The ID can be retrieved with the [crm.item.list](../../../universal/crm-item-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":999, "sourceEntityTypeId":2, "sourceEntityId": 1, "targetEntityTypeId":2, "targetEntityId": 100}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.binding.move
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":999,"sourceEntityTypeId":2,"sourceEntityId":1,"targetEntityTypeId":2,"targetEntityId":100,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.binding.move
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
        method: 'crm.activity.binding.move',
        params: {
          activityId: 999, // activity ID
          sourceEntityTypeId: 2, // type of the CRM object the activity is currently bound to
          sourceEntityId: 1, // ID of the CRM element the activity is currently bound to
          targetEntityTypeId: 2, // type of the CRM object to bind the activity to
          targetEntityId: 100, // ID of the CRM element to bind the activity to
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Binding moved successfully:', result)
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
      async function moveActivityBinding() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.binding.move',
            params: {
              activityId: 999, // activity ID
              sourceEntityTypeId: 2, // type of the CRM object the activity is currently bound to
              sourceEntityId: 1, // ID of the CRM element the activity is currently bound to
              targetEntityTypeId: 2, // type of the CRM object to bind the activity to
              targetEntityId: 100, // ID of the CRM element to bind the activity to
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Binding moved successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', moveActivityBinding)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.binding.move(
            activity_id=999,
            source_entity_type_id=2,
            source_entity_id=1,
            target_entity_type_id=2,
            target_entity_id=100,
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
                'crm.activity.binding.move',
                [
                    'activityId'         => 999,
                    'sourceEntityTypeId' => 2,
                    'sourceEntityId'     => 1,
                    'targetEntityTypeId' => 2,
                    'targetEntityId'     => 100,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        // The data processing logic you need
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error moving activity binding: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.binding.move',
        {
            activityId: 999, // Case ID
            sourceEntityTypeId: 2, // Object type to which the case is linked
            sourceEntityId: 1, // Element ID to which the case is linked
            targetEntityTypeId: 2, // Object type to which the case will be linked
            targetEntityId: 100 // Element ID to which the case will be linked
        },
        function(result) {
            if (result.error()) {
                console.error('Error:', result.error());
            } else {
                console.log('Result:', result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.binding.move',
        [
            'activityId' => 999, // Case ID
            'sourceEntityTypeId' => 2, // Object type to which the case is linked
            'sourceEntityId' => 1, // Element ID to which the case is linked
            'targetEntityTypeId' => 2, // Object type to which the case will be linked
            'targetEntityId' => 100 // Element ID to which the case will be linked
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.binding.move", b24.Params{
    	"activityId":         999,
    	"sourceEntityTypeId": 2,
    	"sourceEntityId":     1,
    	"targetEntityTypeId": 2,
    	"targetEntityId":     100,
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.binding.move: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

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
        "operating_reset_at": 1712133392,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | The result of the operation. Returns `true` if the binding was moved ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
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
|| `400` | `100` | Could not find value for parameter {targetEntityId} | The required parameter `activityId`, `sourceEntityTypeId`, `sourceEntityId`, `targetEntityTypeId`, or `targetEntityId` was not passed. The description contains the parameter name ||
|| `400` | `100` | Invalid value {abc} to match with parameter {activityId}. Should be value of type int. | The parameter value is not an integer. The description contains the parameter name ||
|| `400` | `NOT_FOUND` | Item was not found | The activity with the `activityId` identifier was not found. In cloud Bitrix24, the error also occurs for a non-existent CRM item ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have permission to edit the source or target CRM item ||
|| `400` | `SOURCE_AND_TARGET_ENTITY_TYPES_ARE_NOT_EQUAL_ERROR` | Source and target entity types are not equal | The `sourceEntityTypeId` and `targetEntityTypeId` values differ ||
|| `400` | `SOURCE_AND_TARGET_ENTITY_ID_ARE_EQUAL_ERROR` | Source and target entity ID cannot be equal | The `sourceEntityId` and `targetEntityId` values are the same ||
|| `400` | `ACTIVITY_IS_ALREADY_BOUND` | Activity is already linked to this entity | The activity is already bound to the target item ||
|| `400` | `BINDING_NOT_FOUND` | Activity is not linked to this entity | The activity is not bound to the source item ||
|#

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-binding-add.md)
- [{#T}](./crm-activity-binding-list.md)
- [{#T}](./crm-activity-binding-delete.md)
- [{#T}](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity.md)
- [{#T}](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity-between-objects.md)
- [{#T}](./index.md)
