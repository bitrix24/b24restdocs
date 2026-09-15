# Get the list of resources booking.v1.resource.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resource.list` returns a list of resources based on a filter. It is an implementation of the listing method for resources.

## Method Parameters

All parameters are optional. Without parameters, the first page of all Bitrix24 resources is returned.

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the list of resources in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where
- `field_N` — [field](#filter) of the resource for filtering
- `value_N` — field value

Filter conditions are combined with a logical AND. Fields absent from the list below are ignored by the method without an error ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the list of resources in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where
- `field_N` — [field](#order) of the resource for sorting
- `value_N` — sort direction

The sort direction can take the following values:
- `asc` — ascending
- `desc` — descending

The value is case-insensitive. If the parameter is not provided, the order of records is not guaranteed — set the sorting explicitly ||
|| **start**
[`integer`](../../data-types.md) | A parameter for managing pagination.

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
[`string`](../../data-types.md) | Search query. The method searches for a case-insensitive substring in the resource name. The resource description is not included in the search ||
|| **isMain**
[`string`](../../data-types.md) | Filter by the resource display setting. Possible values:
- `Y` — in the schedule columns
- `N` — when resources overlap

Pass the value as a string. Values of other types, such as `true`, are ignored by the method ||
|| **typeId**
[`integer`](../../data-types.md) \| [`array`](../../data-types.md) | Resource type identifier or an array of identifiers. For an array, the method returns the resources of all listed types.

The list of available types can be retrieved using the [booking.v1.resourceType.list](./resource-type/booking-v1-resourcetype-list.md) method ||
|| **name**
[`string`](../../data-types.md) | Resource name. The method searches for an exact match ||
|| **description**
[`string`](../../data-types.md) | Resource description. The method searches for an exact match ||
|#

The method does not support comparison operators, such as `%name` or `>id`. The other filter fields accept a single value.

### Order Parameters {#order}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Sort by identifier ||
|| **name**
[`string`](../../data-types.md) | Sort by name ||
|#

Sorting by other fields is not supported — such fields are ignored by the method.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"searchQuery":"car","isMain":"Y","typeId":1},"order":{"id":"ASC","name":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resource.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"searchQuery":"car","isMain":"Y","typeId":1},"order":{"id":"ASC","name":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resource.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each resource returned in result.resource[]
    type ResourceItem = {
      confirmationCounterDelay: number
      confirmationNotificationDelay: number
      confirmationNotificationRepetitions: number | null
      confirmationNotificationRepetitionsInterval: number
      delayedCounterDelay: number
      delayedNotificationDelay: number
      description: string | null
      id: number
      infoNotificationDelay: number | null
      isConfirmationNotificationOn: string
      isDelayedNotificationOn: string
      isFeedbackNotificationOn: string
      isInfoNotificationOn: string
      isMain: string
      isReminderNotificationOn: string
      name: string
      reminderNotificationDelay: number
      templateTypeConfirmation: string
      templateTypeDelayed: string
      templateTypeFeedback: string
      templateTypeInfo: string
      templateTypeReminder: string
      typeId: number
    }

    try {
      // The callList.make() and fetchList.make() list helpers do not fit this method:
      // they page by the '>id' cursor, and the method supports neither filtering by id nor operators.
      // Request the next pages via call.make() with start: 50, 100, and so on
      const response = await $b24.actions.v2.call.make<{ resource: ResourceItem[] }>({
        method: 'booking.v1.resource.list',
        params: {
          filter: {
            searchQuery: 'car',
            isMain: 'Y',
            typeId: 1,
          },
          order: {
            id: 'ASC',
            name: 'DESC',
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
        console.info('Resources:', result.resource, 'Count:', result.resource.length)
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
      async function fetchResourceList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // The callList.make() and fetchList.make() list helpers do not fit this method:
          // they page by the '>id' cursor, and the method supports neither filtering by id nor operators.
          // Request the next pages via call.make() with start: 50, 100, and so on
          const response = await $b24.actions.v2.call.make({
            method: 'booking.v1.resource.list',
            params: {
              filter: {
                searchQuery: 'car',
                isMain: 'Y',
                typeId: 1,
              },
              order: {
                id: 'ASC',
                name: 'DESC',
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
          console.info('Resources:', result.resource, 'Count:', result.resource.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchResourceList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException
    try:
        bitrix_response = client.booking.v1.resource.list(filter={
            "typeId": 1,
            "searchQuery": "auto",
            "isMain": "Y",
        }, order={
            "id": "desc",
            "name": "DESC",
        }).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
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
                'booking.v1.resource.list',
                [
                    'filter' => [
                        'searchQuery' => 'car',
                        'isMain'      => 'Y',
                        'typeId'      => 1,
                    ],
                    'order' => [
                        'id'   => 'ASC',
                        'name' => 'DESC',
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
        echo 'Error calling booking.v1.resource.list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.resource.list",
        {
            filter: {
                "searchQuery": "car",
                "isMain": "Y",
                "typeId": 1
            },
            order: {
                id: "ASC",
                name: "DESC"
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
        'booking.v1.resource.list',
        [
            'filter' => [
                'searchQuery' => 'car',
                'isMain' => 'Y',
                'typeId' => 1
            ],
            'order' => [
                'id' => 'ASC',
                'name' => 'DESC'
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
    res, err := client.Core().Call(ctx, "booking.v1.resource.list", b24.Params{
    	"filter": b24.Params{
    		"searchQuery": "car",
    		"isMain":      "Y",
    		"typeId":      1,
    	},
    	"order": b24.Params{
    		"id":   "ASC",
    		"name": "DESC",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("booking.v1.resource.list: %w", err)
    }

    // The method wraps the response in an object with the "resource" key.
    raw, ok := b24.Unwrap(res.Result, "resource")
    if !ok {
    	return fmt.Errorf("no resource key in the response")
    }

    var items []struct {
    	ConfirmationCounterDelay                    int    `json:"confirmationCounterDelay"`
    	ConfirmationNotificationDelay               int    `json:"confirmationNotificationDelay"`
    	ConfirmationNotificationRepetitionsInterval int    `json:"confirmationNotificationRepetitionsInterval"`
    	DelayedCounterDelay                         int    `json:"delayedCounterDelay"`
    	DelayedNotificationDelay                    int    `json:"delayedNotificationDelay"`
    	ID                                          b24.ID `json:"id"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ConfirmationCounterDelay)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "resource": [
            {
                "confirmationCounterDelay": 10800,
                "confirmationNotificationDelay": 86400,
                "confirmationNotificationRepetitions": null,
                "confirmationNotificationRepetitionsInterval": 10800,
                "delayedCounterDelay": 300,
                "delayedNotificationDelay": 300,
                "description": null,
                "id": 5,
                "infoNotificationDelay": null,
                "isConfirmationNotificationOn": "Y",
                "isDelayedNotificationOn": "Y",
                "isFeedbackNotificationOn": "N",
                "isInfoNotificationOn": "Y",
                "isMain": "Y",
                "isReminderNotificationOn": "Y",
                "name": "Sedan 1",
                "reminderNotificationDelay": -1,
                "templateTypeConfirmation": "inanimate",
                "templateTypeDelayed": "inanimate",
                "templateTypeFeedback": "inanimate",
                "templateTypeInfo": "inanimate",
                "templateTypeReminder": "base",
                "typeId": 1
            },
            {
                "confirmationCounterDelay": 10800,
                "confirmationNotificationDelay": 86400,
                "confirmationNotificationRepetitions": null,
                "confirmationNotificationRepetitionsInterval": 10800,
                "delayedCounterDelay": 300,
                "delayedNotificationDelay": 300,
                "description": null,
                "id": 7,
                "infoNotificationDelay": null,
                "isConfirmationNotificationOn": "Y",
                "isDelayedNotificationOn": "Y",
                "isFeedbackNotificationOn": "N",
                "isInfoNotificationOn": "Y",
                "isMain": "Y",
                "isReminderNotificationOn": "Y",
                "name": "Sedan 2",
                "reminderNotificationDelay": -1,
                "templateTypeConfirmation": "inanimate",
                "templateTypeDelayed": "inanimate",
                "templateTypeFeedback": "inanimate",
                "templateTypeInfo": "inanimate",
                "templateTypeReminder": "base",
                "typeId": 1
            }
        ]
    },
    "time": {
        "start": 1746540454.261779,
        "finish": 1746540454.303483,
        "duration": 0.04170393943786621,
        "processing": 0.009412050247192383,
        "date_start": "2025-05-06T17:07:34+02:00",
        "date_finish": "2025-05-06T17:07:34+02:00",
        "operating_reset_at": 1746541054,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains a single field `resource` — an array of objects with information about resources; the object structure is described [below](#resource) ||
|| **total**
[`integer`](../../data-types.md) | Service field. The method always returns `0`, and the `next` field is absent from the response. When called with `start: -1`, the `total` field is absent as well — they cannot be used to iterate over pages ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

The sign of the last page is fewer than 50 records in the response.

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
    "error": "",
    "error_description": "Invalid value {ASC} to match with parameter {order}. Should be value of type array."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Invalid value {value} to match with parameter {order}. Should be value of type array.` | The `order` parameter is not an object ||
|| `100` | `Invalid value {value} to match with parameter {filter}. Should be value of type array.` | The `filter` parameter is not an object ||
|| `100` | `Invalid order "XXX"` | A sort direction other than `asc` or `desc` is passed in the `order` parameter ||
|| `0` | `Booking tool is disabled. Please contact your administrator.` | The Booking tool is disabled in the Bitrix24 settings ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./booking-v1-resource-add.md)
- [{#T}](./booking-v1-resource-update.md)
- [{#T}](./booking-v1-resource-get.md)
- [{#T}](./booking-v1-resource-delete.md)
- [{#T}](./resource-type/index.md)
- [{#T}](./slots/index.md)
