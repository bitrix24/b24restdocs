# Get a list of resource types booking.v1.resourceType.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resourceType.list` returns a list of resource types based on a filter. It is an implementation of the list method for resource types.

## Method Parameters

All parameters are optional. Without parameters, the first page of all resource types is returned.

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the list of resource types in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where
- `field_N` — [field](#filter) of the resource type for filtering
- `value_N` — field value

Filter conditions are combined with a logical AND. Fields absent from the list below are ignored by the method without an error ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the list of resource types in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where
- `field_N` — [field](#order) of the resource type for sorting
- `value_N` — sort direction

The sort direction can take the following values:
- `asc` — ascending
- `desc` — descending

The value is case-insensitive. If the parameter is not provided, the order of records is not guaranteed — set the sorting explicitly ||
|| **start**
[`integer`](../../../data-types.md) | A parameter for managing pagination.

The result page size is always fixed: 50 records.

To select the second page of results, pass the value `50`, to select the third — `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the number of the required page.

A value that is not a multiple of 50 is rounded down to the page boundary: with `start` from `1` to `49`, the first page is returned.

The value `-1` disables the pagination count — the `total` field is absent from the response.

The default value is `0` ||
|#

The method recognizes parameter and field names only in the form given in the tables: entries such as `FILTER` or `SEARCH_QUERY` are ignored.

### Filter Parameters {#filter}

#|
|| **Name**
`type` | **Description** ||
|| **searchQuery**
[`string`](../../../data-types.md) | Search query. The method searches for a case-insensitive substring in the resource type name ||
|| **moduleId**
[`string`](../../../data-types.md) | Identifier of the module the resource type belongs to. For types created via REST, the value is `booking`.

Without this filter, the method returns the types of all modules ||
|| **name**
[`string`](../../../data-types.md) | Resource type name. The method searches for an exact match ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code of the resource type. The method searches for an exact match ||
|#

The method does not support filtering by identifier: to retrieve a single type by `id`, use [booking.v1.resourceType.get](./booking-v1-resourcetype-get.md). Comparison operators, such as `%name`, are not supported either.

### Order Parameters {#order}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Sort by identifier ||
|| **name**
[`string`](../../../data-types.md) | Sort by name ||
|| **code**
[`string`](../../../data-types.md) | Sort by code ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"searchQuery":"res","moduleId":"booking"},"order":{"id":"ASC","name":"DESC","code":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resourceType.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"searchQuery":"res","moduleId":"booking"},"order":{"id":"ASC","name":"DESC","code":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resourceType.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ResourceTypeResult = {
      resourceType: ResourceType[]
    }

    type ResourceType = {
      code: string
      confirmationCounterDelay: number
      confirmationNotificationDelay: number
      confirmationNotificationRepetitions: number | null
      confirmationNotificationRepetitionsInterval: number
      delayedCounterDelay: number
      delayedNotificationDelay: number
      id: number
      infoNotificationDelay: number | null
      isConfirmationNotificationOn: string
      isDelayedNotificationOn: string
      isFeedbackNotificationOn: string
      isReminderNotificationOn: string
      name: string
      reminderNotificationDelay: number
      templateTypeConfirmation: string
      templateTypeDelayed: string
      templateTypeFeedback: string
      templateTypeReminder: string
    }

    try {
      // booking.v1.resourceType.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ResourceTypeResult>({
        method: 'booking.v1.resourceType.list',
        params: {
          filter: {
            searchQuery: 'res',
            moduleId: 'booking',
          },
          order: {
            id: 'ASC',
            name: 'DESC',
            code: 'DESC',
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
        console.info('Resource types:', result.resourceType.length, result.resourceType)
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
      async function listResourceTypes() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // booking.v1.resourceType.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'booking.v1.resourceType.list',
            params: {
              filter: {
                searchQuery: 'res',
                moduleId: 'booking',
              },
              order: {
                id: 'ASC',
                name: 'DESC',
                code: 'DESC',
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
          console.info('Resource types:', result.resourceType.length, result.resourceType)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listResourceTypes)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.booking.v1.resource_type.list(
            filter={
                "searchQuery": "res",
                "moduleId": "booking",
            },
            order={
                "id": "ASC",
                "name": "DESC",
                "code": "DESC",
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
                'booking.v1.resourceType.list',
                [
                    'filter' => [
                        'searchQuery' => 'res',
                        'moduleId'    => 'booking',
                    ],
                    'order'  => [
                        'id'   => 'ASC',
                        'name' => 'DESC',
                        'code' => 'DESC',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing resource types: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.resourceType.list",
        {
            filter: {
                    "searchQuery": "res",
                    "moduleId": "booking"
        },
        order: {
                id: "ASC",
                name: "DESC",
                code: "DESC"
            }
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
        'booking.v1.resourceType.list',
        [
            'filter' => [
                'searchQuery' => 'res',
                'moduleId' => 'booking'
            ],
            'order' => [
                'id' => 'ASC',
                'name' => 'DESC',
                'code' => 'DESC'
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
    res, err := client.Core().Call(ctx, "booking.v1.resourceType.list", b24.Params{
    	"FILTER": b24.Params{
    		"searchQuery": "res",
    		"moduleId":    "booking",
    	},
    	"ORDER": b24.Params{
    		"id":   "ASC",
    		"name": "DESC",
    		"code": "DESC",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("booking.v1.resourceType.list: %w", err)
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
    "result": {
        "resourceType": [
            {
                "cancellationNotificationDelay": 600,
                "code": "equipment",
                "confirmationCounterDelay": 10800,
                "confirmationNotificationDelay": 86400,
                "confirmationNotificationRepetitions": null,
                "confirmationNotificationRepetitionsInterval": 10800,
                "delayedCounterDelay": 300,
                "delayedNotificationDelay": 300,
                "id": 3,
                "infoNotificationDelay": null,
                "isCancellationNotificationOn": "Y",
                "isConfirmationNotificationOn": "Y",
                "isDelayedNotificationOn": "Y",
                "isFeedbackNotificationOn": "N",
                "isInfoNotificationOn": "Y",
                "isReminderNotificationOn": "Y",
                "name": "Equipment",
                "reminderNotificationDelay": -1,
                "senderCode": null,
                "templateTypeConfirmation": "inanimate",
                "templateTypeDelayed": "inanimate",
                "templateTypeFeedback": "inanimate",
                "templateTypeReminder": "base"
            },
            {
                "cancellationNotificationDelay": 600,
                "code": "expert",
                "confirmationCounterDelay": 10800,
                "confirmationNotificationDelay": 86400,
                "confirmationNotificationRepetitions": null,
                "confirmationNotificationRepetitionsInterval": 10800,
                "delayedCounterDelay": 300,
                "delayedNotificationDelay": 300,
                "id": 5,
                "infoNotificationDelay": null,
                "isCancellationNotificationOn": "Y",
                "isConfirmationNotificationOn": "Y",
                "isDelayedNotificationOn": "Y",
                "isFeedbackNotificationOn": "N",
                "isInfoNotificationOn": "Y",
                "isReminderNotificationOn": "Y",
                "name": "Specialist",
                "reminderNotificationDelay": -1,
                "senderCode": null,
                "templateTypeConfirmation": "inanimate",
                "templateTypeDelayed": "inanimate",
                "templateTypeFeedback": "inanimate",
                "templateTypeReminder": "base"
            }
        ]
    },
    "total": 0,
    "time": {
        "start": 1746540063.20403,
        "finish": 1746540063.261006,
        "duration": 0.0569760799407959,
        "processing": 0.020888090133666992,
        "date_start": "2025-05-06T17:01:03+02:00",
        "date_finish": "2025-05-06T17:01:03+02:00",
        "operating_reset_at": 1746540663,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The root element of the response. Contains a single field `resourceType` — an array of objects with information about resource types; the object structure is described [below](#resource) ||
|| **total**
[`integer`](../../../data-types.md) | Service field. The method always returns `0`, and the `next` field is absent from the response — they cannot be used to iterate over pages ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

The sign of the last page is fewer than 50 records in the response.

#### Resource Type {#resource}

Numeric and string fields are returned as `null` when no value is set. For example, a notification delay of `0` arrives as `null` in the response. The `is*` flags and the `templateType*` fields are always populated.

#|
|| **Name**
`type` | **Description** ||
|| **cancellationNotificationDelay**
[`integer`](../../../data-types.md) | Time in seconds after a booking is cancelled, after which the client receives the cancellation message ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code of the resource type. Unique within the module ||
|| **confirmationCounterDelay**
[`integer`](../../../data-types.md) | Time in seconds before the booking, after which the unconfirmed booking counter is activated ||
|| **confirmationNotificationDelay**
[`integer`](../../../data-types.md) | Time in seconds before the booking, when the client receives the first confirmation message ||
|| **confirmationNotificationRepetitions**
[`integer`](../../../data-types.md) | Number of confirmation messages sent to the client, excluding the first one ||
|| **confirmationNotificationRepetitionsInterval**
[`integer`](../../../data-types.md) | Interval between booking confirmation messages, in seconds ||
|| **delayedCounterDelay**
[`integer`](../../../data-types.md) | Time in seconds after which the counter is activated in the calendar ||
|| **delayedNotificationDelay**
[`integer`](../../../data-types.md) | Time in seconds after which the late arrival message is sent to the client ||
|| **id**
[`integer`](../../../data-types.md) | Resource type identifier ||
|| **infoNotificationDelay**
[`integer`](../../../data-types.md) | Time in seconds after which the client receives the booking message ||
|| **isCancellationNotificationOn**
[`string`](../../../data-types.md) | Message to the client after a booking is cancelled. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isConfirmationNotificationOn**
[`string`](../../../data-types.md) | Message to the client requesting booking confirmation. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isDelayedNotificationOn**
[`string`](../../../data-types.md) | Reminder when the client is running late. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isFeedbackNotificationOn**
[`string`](../../../data-types.md) | Feedback request. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isInfoNotificationOn**
[`string`](../../../data-types.md) | Booking message to the client. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **isReminderNotificationOn**
[`string`](../../../data-types.md) | Booking reminder. Possible values:
- `Y` — enabled
- `N` — disabled ||
|| **name**
[`string`](../../../data-types.md) | Resource type name ||
|| **reminderNotificationDelay**
[`integer`](../../../data-types.md) | Time in seconds before the booking, at which the client receives the reminder.

The value `-1` means the reminder arrives on the morning of the booking day ||
|| **senderCode**
[`string`](../../../data-types.md) | Code of the service that sends messages to the client. Possible values:
- `bitrix24` — Bitrix24 notifications
- `ai_call` — AI agent call ||
|| **templateTypeConfirmation**
[`string`](../../../data-types.md) | Message template type for booking confirmation. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking with specialists
- `inanimate_long` — template for multi-day booking ||
|| **templateTypeDelayed**
[`string`](../../../data-types.md) | Message template type for late arrival. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking with specialists ||
|| **templateTypeFeedback**
[`string`](../../../data-types.md) | Message template type for the feedback request. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking with specialists ||
|| **templateTypeReminder**
[`string`](../../../data-types.md) | Message template type for the reminder. The only value is `base` ||
|#

The resource type does not return the `templateTypeInfo` field: the booking message template of a resource type is not available via REST. The resource itself does have this field — see [booking.v1.resource.get](../booking-v1-resource-get.md).

The method does not return `moduleId` either — the response does not indicate which module the type belongs to. The method returns a type by any `id`, including types of other modules. To retrieve only the types of the `booking` module, use the `moduleId` filter in the [booking.v1.resourceType.list](./booking-v1-resourcetype-list.md) method.

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Invalid value {ASC} to match with parameter {order}. Should be value of type array."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Invalid value {value} to match with parameter {order}. Should be value of type array.` | The `order` parameter is not an object ||
|| `100` | `Invalid value {value} to match with parameter {filter}. Should be value of type array.` | The `filter` parameter is not an object ||
|| `100` | `Invalid order "XXX"` | A sort direction other than `asc` or `desc` is passed in the `order` parameter ||
|| `0` | `Booking tool is disabled. Please contact your administrator.` | The Booking tool is disabled in the Bitrix24 settings ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./booking-v1-resourcetype-add.md)
- [{#T}](./booking-v1-resourcetype-update.md)
- [{#T}](./booking-v1-resourcetype-get.md)
- [{#T}](./booking-v1-resourcetype-delete.md)
- [{#T}](../index.md)
