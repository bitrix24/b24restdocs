# Get resource booking.v1.resource.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resource.get` returns information about a resource by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Resource identifier.

Can be obtained from the methods [booking.v1.resource.add](./booking-v1-resource-add.md) and [booking.v1.resource.list](./booking-v1-resource-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resource.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resource.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ResourceResult = {
      resource: {
        cancellationNotificationDelay: number | null,
        confirmationCounterDelay: number | null,
        confirmationNotificationDelay: number | null,
        confirmationNotificationRepetitions: number | null,
        confirmationNotificationRepetitionsInterval: number | null,
        delayedCounterDelay: number | null,
        delayedNotificationDelay: number | null,
        description: string | null,
        id: number,
        infoNotificationDelay: number | null,
        isCancellationNotificationOn: string,
        isConfirmationNotificationOn: string,
        isDelayedNotificationOn: string,
        isFeedbackNotificationOn: string,
        isInfoNotificationOn: string,
        isMain: string,
        isReminderNotificationOn: string,
        name: string | null,
        reminderNotificationDelay: number | null,
        senderCode: string,
        templateTypeConfirmation: string,
        templateTypeDelayed: string,
        templateTypeFeedback: string,
        templateTypeInfo: string,
        templateTypeReminder: string,
        typeId: number,
      },
    }

    try {
      const response = await $b24.actions.v2.call.make<ResourceResult>({
        method: 'booking.v1.resource.get',
        params: {
          id: 15,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.resource.id, result.resource.name, result.resource.typeId)
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
      async function getResource() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'booking.v1.resource.get',
            params: {
              id: 15,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.resource.id, result.resource.name, result.resource.typeId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getResource)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.booking.v1.resource.get(
            bitrix_id=15,
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
                'booking.v1.resource.get',
                [
                    'id' => 15,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting resource: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.resource.get",
        {
            id: 15
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
        'booking.v1.resource.get',
        [
            'id' => 15
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "booking.v1.resource.get", b24.Params{
    	"id": 15,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("booking.v1.resource.get: %w", err)
    }

    // The method wraps the response in an object with the "resource" key.
    raw, ok := b24.Unwrap(res.Result, "resource")
    if !ok {
    	return fmt.Errorf("no resource key in the response")
    }

    var item struct {
    	ConfirmationCounterDelay                    int    `json:"confirmationCounterDelay"`
    	ConfirmationNotificationDelay               int    `json:"confirmationNotificationDelay"`
    	ConfirmationNotificationRepetitionsInterval int    `json:"confirmationNotificationRepetitionsInterval"`
    	DelayedCounterDelay                         int    `json:"delayedCounterDelay"`
    	DelayedNotificationDelay                    int    `json:"delayedNotificationDelay"`
    	ID                                          b24.ID `json:"id"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ConfirmationCounterDelay, item.ConfirmationNotificationDelay)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "resource": {
            "cancellationNotificationDelay": 600,
            "confirmationCounterDelay": 10800,
            "confirmationNotificationDelay": 86400,
            "confirmationNotificationRepetitions": null,
            "confirmationNotificationRepetitionsInterval": 10800,
            "delayedCounterDelay": 300,
            "delayedNotificationDelay": 300,
            "description": null,
            "id": 15,
            "infoNotificationDelay": null,
            "isCancellationNotificationOn": "Y",
            "isConfirmationNotificationOn": "Y",
            "isDelayedNotificationOn": "N",
            "isFeedbackNotificationOn": "N",
            "isInfoNotificationOn": "Y",
            "isMain": "Y",
            "isReminderNotificationOn": "Y",
            "name": "Name",
            "reminderNotificationDelay": -1,
            "senderCode": "bitrix24",
            "templateTypeConfirmation": "animate",
            "templateTypeDelayed": "animate",
            "templateTypeFeedback": "animate",
            "templateTypeInfo": "inanimate",
            "templateTypeReminder": "base",
            "typeId": 1
        }
    },
    "time": {
        "start": 1746539524.292041,
        "finish": 1746539524.356627,
        "duration": 0.06458592414855957,
        "processing": 0.018703937530517578,
        "date_start": "2025-05-06T16:52:04+02:00",
        "date_finish": "2025-05-06T16:52:04+02:00",
        "operating_reset_at": 1746540124,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains a single field `resource` — an object with information about the resource; the structure is described [below](#resource) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Resource {#resource}

Numeric and string fields are returned as `null` when no value is set. For example, a notification delay of `0` arrives as `null` in the response. The `is*` flags, the `templateType*` fields, and `senderCode` are always populated.

#|
|| **Name**
`type` | **Description** ||
|| **cancellationNotificationDelay**
[`integer`](../../data-types.md) | Time in seconds after a booking is cancelled, after which the client receives the cancellation message ||
|| **confirmationCounterDelay**
[`integer`](../../data-types.md) | Time in seconds before the booking, after which the unconfirmed booking counter is activated ||
|| **confirmationNotificationDelay**
[`integer`](../../data-types.md) | Time in seconds before the booking, when the client receives the first confirmation message ||
|| **confirmationNotificationRepetitions**
[`integer`](../../data-types.md) | Number of confirmation messages sent to the client, excluding the first one ||
|| **confirmationNotificationRepetitionsInterval**
[`integer`](../../data-types.md) | Interval between booking confirmation messages, in seconds ||
|| **delayedCounterDelay**
[`integer`](../../data-types.md) | Time in seconds after which the counter is activated in the calendar ||
|| **delayedNotificationDelay**
[`integer`](../../data-types.md) | Time in seconds after which the late arrival message is sent to the client ||
|| **description**
[`string`](../../data-types.md) | Resource description ||
|| **id**
[`integer`](../../data-types.md) | Resource identifier ||
|| **infoNotificationDelay**
[`integer`](../../data-types.md) | Time in seconds after which the client receives the booking message ||
|| **isCancellationNotificationOn**
[`string`](../../data-types.md) | Message to the client after a booking is cancelled. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isConfirmationNotificationOn**
[`string`](../../data-types.md) | Message to the client requesting booking confirmation. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isDelayedNotificationOn**
[`string`](../../data-types.md) | Reminder when the client is running late. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isFeedbackNotificationOn**
[`string`](../../data-types.md) | Feedback request. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isInfoNotificationOn**
[`string`](../../data-types.md) | Booking message to the client. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isMain**
[`string`](../../data-types.md) | How the resource is displayed. Possible values:
- `Y` — in the schedule columns
- `N` — when resources overlap ||
|| **isReminderNotificationOn**
[`string`](../../data-types.md) | Booking reminder. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **name**
[`string`](../../data-types.md) | Resource name ||
|| **reminderNotificationDelay**
[`integer`](../../data-types.md) | Time in seconds before the booking, at which the client receives the reminder.

The value `-1` means the reminder arrives on the morning of the booking day ||
|| **senderCode**
[`string`](../../data-types.md) | Code of the service that sends messages to the client. Possible values:
- `bitrix24` — Bitrix24 notifications
- `ai_call` — AI agent call ||
|| **templateTypeConfirmation**
[`string`](../../data-types.md) | Message template type for booking confirmation. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking with specialists
- `inanimate_long` — template for multi-day booking ||
|| **templateTypeDelayed**
[`string`](../../data-types.md) | Message template type for late arrival. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking with specialists ||
|| **templateTypeFeedback**
[`string`](../../data-types.md) | Message template type for the feedback request. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking with specialists ||
|| **templateTypeInfo**
[`string`](../../data-types.md) | Message template type for the booking message. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking with specialists
- `inanimate_long` — template for multi-day booking ||
|| **templateTypeReminder**
[`string`](../../data-types.md) | Message template type for the reminder. The only value is `base` ||
|| **typeId**
[`integer`](../../data-types.md) | Resource type identifier.

Information about the type can be retrieved using the [booking.v1.resourceType.get](./resource-type/booking-v1-resourcetype-get.md) method ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "1009",
    "error_description": "Resource not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `1009` | `Resource not found` | Resource with such `id` not found ||
|| `100` | `Could not find value for parameter {id}` | Required parameter not provided ||
|| `0` | `Booking tool is disabled. Please contact your administrator.` | The Booking tool is disabled in the Bitrix24 settings ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./booking-v1-resource-add.md)
- [{#T}](./booking-v1-resource-update.md)
- [{#T}](./booking-v1-resource-list.md)
- [{#T}](./booking-v1-resource-delete.md)
- [{#T}](./resource-type/index.md)
- [{#T}](./slots/index.md)
