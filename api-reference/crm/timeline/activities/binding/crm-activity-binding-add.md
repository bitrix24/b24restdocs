# Add a deal binding to a CRM object crm.activity.binding.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM item the activity is being bound to and permission to read at least one item the activity is already bound to

The method `crm.activity.binding.add` adds a binding of an activity to a CRM item. After that, the activity appears in the item's timeline.

In REST 3.0, a binding is added by the [crm.activity.binding.bind](../../../rest-v3/activities/crm-activity-binding-bind-rest-v3.md) method: the item is passed as a single `parent` object, and the response contains the activity bindings.

{% note warning "Limit" %}

By default, an activity can be bound to no more than 100 CRM items. If you add a binding beyond the limit, the method returns `true` but deletes the binding to the item with the lowest ID. The activity disappears from that item's timeline. The deleted binding may also be the one you have just added.

Before adding a binding, check the number of activity bindings with the [crm.activity.binding.list](./crm-activity-binding-list.md) method. The method shows only bindings to items the user can read, so the number of bindings in its response may be lower than the actual number. After adding, use the same method to make sure the new binding is in the list.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **activityId***
[`integer`](../../../../data-types.md) | Integer identifier of the activity in the timeline, for example `999`.

The ID can be retrieved with the [crm.activity.list](../activity-base/crm-activity-list.md) method ||
|| **entityTypeId***
[`integer`](../../../../data-types.md) | [Integer identifier of the CRM object type](../../../data-types.md#object_type) to which the activity should be bound, for example `2` for a deal ||
|| **entityId***
[`integer`](../../../../data-types.md) | Integer identifier of the CRM item to which the activity should be bound, for example `1`.

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
    -d '{"activityId":999, "entityTypeId":2, "entityId": 1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.binding.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":999, "entityTypeId":2, "entityId": 1, "auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.binding.add
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
        method: 'crm.activity.binding.add',
        params: {
          activityId: 999,
          entityTypeId: 2,
          entityId: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Binding added:', result)
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
      async function addActivityBinding() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.binding.add',
            params: {
              activityId: 999,
              entityTypeId: 2,
              entityId: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Binding added:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addActivityBinding)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.binding.add(
            activity_id=999,
            entity_type_id=2,
            entity_id=1,
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
                'crm.activity.binding.add',
                [
                    'activityId'   => 999, // Deal ID
                    'entityTypeId' => 2, // CRM object type ID
                    'entityId'     => 1 // CRM object ID
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
        echo 'Error adding activity binding: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.binding.add',
        {
            activityId: 999, // Deal ID
            entityTypeId: 2, // CRM object type ID
            entityId: 1 // CRM object ID
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
        'crm.activity.binding.add',
        [
            'activityId' => 999, // Deal ID
            'entityTypeId' => 2, // CRM object type ID
            'entityId' => 1 // CRM object ID
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.binding.add", b24.Params{
    	"activityId":   999,
    	"entityTypeId": 2,
    	"entityId":     1,
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.binding.add: %w", err)
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
[`boolean`](../../../../data-types.md) | Result of the operation. Returns `true` if Bitrix24 updated the activity bindings. When the limit is exceeded, `true` does not guarantee that the new binding was retained ||
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

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {entityId} | The required parameter `activityId`, `entityTypeId`, or `entityId` was not passed. The description contains the parameter name ||
|| `400` | `100` | Invalid value {abc} to match with parameter {activityId}. Should be value of type int. | The parameter value is not an integer. The description contains the parameter name ||
|| `400` | `NOT_FOUND` | Item was not found | The activity with the `activityId` identifier was not found. In cloud Bitrix24, the error also occurs for a non-existent CRM item ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have permission to edit the CRM item or cannot read any of the items the activity is already bound to ||
|| `400` | `ACTIVITY_IS_ALREADY_BOUND` | Activity is already linked to this entity | The activity is already bound to this CRM item ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-binding-move.md)
- [{#T}](./crm-activity-binding-list.md)
- [{#T}](./crm-activity-binding-delete.md)
- [{#T}](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity-between-objects.md)
- [{#T}](../../../rest-v3/activities/crm-activity-binding-bind-rest-v3.md)
- [{#T}](./index.md)