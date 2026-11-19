# Pin a Timeline Entry crm.timeline.item.pin

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM item

The `crm.timeline.item.pin` method pins an entry in a CRM item timeline.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the timeline item, for example `999`. This is not an activity ID. You can retrieve the `id` using the [crm.timeline.comment.list](../comments/crm-timeline-comment-list.md) and [crm.timeline.logmessage.list](../logmessage/crm-timeline-logmessage-list.md) methods or from the response of [crm.timeline.comment.add](../comments/crm-timeline-comment-add.md) and [crm.timeline.logmessage.add](../logmessage/crm-timeline-logmessage-add.md) ||
|| **ownerTypeId***
[`integer`](../../../data-types.md) | [Identifier of the CRM object type](../../data-types.md#object_type) to which the item is linked, for example `2` for a deal ||
|| **ownerId***
[`integer`](../../../data-types.md) | Identifier of the CRM element to which the item is linked, for example `10`. If the item is linked to several CRM elements, it is pinned only in this element ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id": 999, "ownerTypeId": 2, "ownerId": 10}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.item.pin
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"ownerTypeId":2,"ownerId":10,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.item.pin
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<null>({
        method: 'crm.timeline.item.pin',
        params: {
          id: 999,
          ownerTypeId: 2,
          ownerId: 10,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Pin result:', result)
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
      async function pinTimelineItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.timeline.item.pin',
            params: {
              id: 999,
              ownerTypeId: 2,
              ownerId: 10,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Pin result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', pinTimelineItem)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.timeline.item.pin(
            bitrix_id=999,
            owner_type_id=2,
            owner_id=10,
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
                'crm.timeline.item.pin',
                [
                    'id'          => 999,
                    'ownerTypeId' => 2,
                    'ownerId'     => 10,
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
        "crm.timeline.item.pin",
        {
            id: 999,
            ownerTypeId: 2,
            ownerId: 10,
        }, result => {
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
        'crm.timeline.item.pin',
        [
            'id' => 999,
            'ownerTypeId' => 2,
            'ownerId' => 10
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.timeline.item.pin", b24.Params{
    	"id":          999,
    	"ownerTypeId": 2,
    	"ownerId":     10,
    })
    if err != nil {
    	return fmt.Errorf("crm.timeline.item.pin: %w", err)
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
    "result": null,
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
[`null`](../../../data-types.md) | Result of the operation. Returns `null`, including when the method is called again for an entry that is already pinned, if fewer than seven entries are pinned in the item ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Item was not found."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {id} | A required parameter `id`, `ownerTypeId`, or `ownerId` is not passed. The error text contains the parameter name ||
|| `400` | `100` | Invalid value {abc} to match with parameter {id}. Should be value of type int. | The parameter value is not a number ||
|| `400` | `OWNER_NOT_FOUND` | Owner was not found | An unknown `ownerTypeId` is passed, or the `ownerId` value is less than or equal to `0` ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have permission to edit the CRM item. For a task activity record, the error is also returned if the task is not completed or has been deleted. This check runs after the check that the record is linked to the CRM item and before the pin limit check. Log records about a task, for example "Task created", can be pinned regardless of the task status ||
|| `400` | `NOT_FOUND` | Item was not found | There is no record with the passed `id`, or it is not linked to the CRM item passed in `ownerTypeId` and `ownerId` ||
|| `400` | `0` | You can pin up to 7 activities | Seven records are already pinned in the CRM item. The error is also returned when the method is called again for a record that is already one of the seven pinned. Unpin one of the records using the [crm.timeline.item.unpin](./crm-timeline-item-unpin.md) method ||
|| `400` | `CAN_NOT_CHANGE_PINNED` | Database error text | Failed to save the pin to the database ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-item-unpin.md)
- [{#T}](../comments/crm-timeline-comment-list.md)
- [{#T}](../logmessage/crm-timeline-logmessage-list.md)
