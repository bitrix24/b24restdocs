# Add a New Resource booking.v1.resource.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.resource.add` adds a new resource.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object containing field values for creating a resource [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Resource name ||
|| **description**
[`string`](../../data-types.md) | Resource description.

By default, the field is empty ||
|| **typeId***
[`integer`](../../data-types.md) | Resource type identifier.

The list of available types can be retrieved using the [booking.v1.resourceType.list](./resource-type/booking-v1-resourcetype-list.md) method ||
|| **isMain**
[`string`](../../data-types.md) | How the resource is displayed. Possible values:
- `Y` — in the schedule columns
- `N` — when resources overlap

Default is `Y` ||
|| **isInfoNotificationOn**
[`string`](../../data-types.md) | Booking message to the client. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeInfo**
[`string`](../../data-types.md) | Message template type for the booking message. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking with specialists
- `inanimate_long` — template for multi-day booking

Default is `inanimate` ||
|| **isConfirmationNotificationOn**
[`string`](../../data-types.md) | Message to the client requesting booking confirmation. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeConfirmation**
[`string`](../../data-types.md) | Message template type for booking confirmation. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking with specialists
- `inanimate_long` — template for multi-day booking

Default is `inanimate` ||
|| **isReminderNotificationOn**
[`string`](../../data-types.md) | Booking reminder. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeReminder**
[`string`](../../data-types.md) | Message template type for the reminder. The only value is `base` ||
|| **isFeedbackNotificationOn**
[`string`](../../data-types.md) | Feedback request. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeFeedback**
[`string`](../../data-types.md) | Message template type for the feedback request. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking with specialists

Default is `inanimate` ||
|| **isDelayedNotificationOn**
[`string`](../../data-types.md) | Reminder when the client is running late. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **templateTypeDelayed**
[`string`](../../data-types.md) | Message template type for late arrival. Possible values:
- `inanimate` — template for booking equipment and rooms
- `animate` — template for booking with specialists

Default is `inanimate` ||
|| **isCancellationNotificationOn**
[`string`](../../data-types.md) | Message to the client after a booking is cancelled. Possible values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **cancellationNotificationDelay**
[`integer`](../../data-types.md) | Time after a booking is cancelled, after which the client receives the cancellation message. Specified in seconds.

Default is 600 ||
|| **infoNotificationDelay**
[`integer`](../../data-types.md) | Time after which the client receives the booking message. Specified in seconds.

Default is 0 ||
|| **reminderNotificationDelay**
[`integer`](../../data-types.md) | Time before the booking, at which the client receives the reminder. Specified in seconds.

The value `-1` means the reminder arrives on the morning of the booking day.

Default is `-1` ||
|| **delayedNotificationDelay**
[`integer`](../../data-types.md) | Time after which the late arrival message is sent to the client. Specified in seconds.

Default is 300 ||
|| **delayedCounterDelay**
[`integer`](../../data-types.md) | Time after which the counter is activated in the calendar. Specified in seconds.

Default is 300 ||
|| **confirmationNotificationDelay**
[`integer`](../../data-types.md) | Time before the booking, when the client receives the first confirmation message. Specified in seconds.

Default is 86400 ||
|| **confirmationNotificationRepetitions**
[`integer`](../../data-types.md) | Number of confirmation messages sent to the client, excluding the first one.

Default is 0 ||
|| **confirmationNotificationRepetitionsInterval**
[`integer`](../../data-types.md) | Interval between booking confirmation messages. Specified in seconds.

Default is 10800 ||
|| **confirmationCounterDelay**
[`integer`](../../data-types.md) | Time before the booking, after which the unconfirmed booking counter is activated. Specified in seconds.

Default is 10800 ||
|| **senderCode**
[`string`](../../data-types.md) | Code of the service that sends messages to the client. Possible values:
- `bitrix24` — Bitrix24 notifications
- `ai_call` — AI agent call

The method does not validate the value: the codes listed are the ones Bitrix24 supports.

Default is `bitrix24` ||
|#

Fields with the `Y` and `N` flags accept strings only. The method silently ignores values of other types, such as `true`, and fields absent from the table.

A resource does not inherit notification configurations from its type: when a resource is created, the fields you did not pass receive the default values from the table above.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Name","description":"Description","typeId":1,"isMain":"N","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"N","templateTypeReminder":"base","isFeedbackNotificationOn":"Y","templateTypeFeedback":"inanimate","isDelayedNotificationOn":"Y","templateTypeDelayed":"inanimate","infoNotificationDelay":60,"reminderNotificationDelay":-1,"delayedNotificationDelay":300,"delayedCounterDelay":7200,"confirmationNotificationDelay":86400,"confirmationNotificationRepetitions":1,"confirmationNotificationRepetitionsInterval":3600,"confirmationCounterDelay":7200}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.resource.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Name","description":"Description","typeId":1,"isMain":"N","isInfoNotificationOn":"Y","templateTypeInfo":"inanimate","isConfirmationNotificationOn":"Y","templateTypeConfirmation":"animate","isReminderNotificationOn":"N","templateTypeReminder":"base","isFeedbackNotificationOn":"Y","templateTypeFeedback":"inanimate","isDelayedNotificationOn":"Y","templateTypeDelayed":"inanimate","infoNotificationDelay":60,"reminderNotificationDelay":-1,"delayedNotificationDelay":300,"delayedCounterDelay":7200,"confirmationNotificationDelay":86400,"confirmationNotificationRepetitions":1,"confirmationNotificationRepetitionsInterval":3600,"confirmationCounterDelay":7200},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.resource.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // The method returns the identifier of the created resource as a number, without a wrapper object
    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'booking.v1.resource.add',
        params: {
          fields: {
            name: 'Name',
            description: 'Description',
            typeId: 1,
            isMain: 'N',
            isInfoNotificationOn: 'Y',
            templateTypeInfo: 'inanimate',
            isConfirmationNotificationOn: 'Y',
            templateTypeConfirmation: 'animate',
            isReminderNotificationOn: 'N',
            templateTypeReminder: 'base',
            isFeedbackNotificationOn: 'Y',
            templateTypeFeedback: 'inanimate',
            isDelayedNotificationOn: 'Y',
            templateTypeDelayed: 'inanimate',
            infoNotificationDelay: 60,
            reminderNotificationDelay: -1,
            delayedNotificationDelay: 300,
            delayedCounterDelay: 7200,
            confirmationNotificationDelay: 86400,
            confirmationNotificationRepetitions: 1,
            confirmationNotificationRepetitionsInterval: 3600,
            confirmationCounterDelay: 7200,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('New resource ID:', result)
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
      async function addResource() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'booking.v1.resource.add',
            params: {
              fields: {
                name: 'Name',
                description: 'Description',
                typeId: 1,
                isMain: 'N',
                isInfoNotificationOn: 'Y',
                templateTypeInfo: 'inanimate',
                isConfirmationNotificationOn: 'Y',
                templateTypeConfirmation: 'animate',
                isReminderNotificationOn: 'N',
                templateTypeReminder: 'base',
                isFeedbackNotificationOn: 'Y',
                templateTypeFeedback: 'inanimate',
                isDelayedNotificationOn: 'Y',
                templateTypeDelayed: 'inanimate',
                infoNotificationDelay: 60,
                reminderNotificationDelay: -1,
                delayedNotificationDelay: 300,
                delayedCounterDelay: 7200,
                confirmationNotificationDelay: 86400,
                confirmationNotificationRepetitions: 1,
                confirmationNotificationRepetitionsInterval: 3600,
                confirmationCounterDelay: 7200,
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
          console.info('New resource ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addResource)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.booking.v1.resource.add(
            fields={
                "name": "Name",
                "description": "Description",
                "typeId": 1,
                "isMain": "N",
                "isInfoNotificationOn": "Y",
                "templateTypeInfo": "inanimate",
                "isConfirmationNotificationOn": "Y",
                "templateTypeConfirmation": "animate",
                "isReminderNotificationOn": "N",
                "templateTypeReminder": "base",
                "isFeedbackNotificationOn": "Y",
                "templateTypeFeedback": "inanimate",
                "isDelayedNotificationOn": "Y",
                "templateTypeDelayed": "inanimate",
                "infoNotificationDelay": 60,
                "reminderNotificationDelay": -1,
                "delayedNotificationDelay": 300,
                "delayedCounterDelay": 7200,
                "confirmationNotificationDelay": 86400,
                "confirmationNotificationRepetitions": 1,
                "confirmationNotificationRepetitionsInterval": 3600,
                "confirmationCounterDelay": 7200,
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
                'booking.v1.resource.add',
                [
                    'fields' => [
                        'name' => 'Name',
                        'description' => 'Description',
                        'typeId' => 1,
                        'isMain' => 'N',
                        'isInfoNotificationOn' => 'Y',
                        'templateTypeInfo' => 'inanimate',
                        'isConfirmationNotificationOn' => 'Y',
                        'templateTypeConfirmation' => 'animate',
                        'isReminderNotificationOn' => 'N',
                        'templateTypeReminder' => 'base',
                        'isFeedbackNotificationOn' => 'Y',
                        'templateTypeFeedback' => 'inanimate',
                        'isDelayedNotificationOn' => 'Y',
                        'templateTypeDelayed' => 'inanimate',
                        'infoNotificationDelay' => 60,
                        'reminderNotificationDelay' => -1,
                        'delayedNotificationDelay' => 300,
                        'delayedCounterDelay' => 7200,
                        'confirmationNotificationDelay' => 86400,
                        'confirmationNotificationRepetitions' => 1,
                        'confirmationNotificationRepetitionsInterval' => 3600,
                        'confirmationCounterDelay' => 7200,
                    ],
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
        echo 'Error adding resource: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.resource.add",
        {
            fields: {
                name: "Name",
                description: "Description",
                typeId: 1,
                isMain: "N",
                isInfoNotificationOn: "Y",
                templateTypeInfo: "inanimate",
                isConfirmationNotificationOn: "Y",
                templateTypeConfirmation: "animate",
                isReminderNotificationOn: "N",
                templateTypeReminder: "base",
                isFeedbackNotificationOn: "Y",
                templateTypeFeedback: "inanimate",
                isDelayedNotificationOn: "Y",
                templateTypeDelayed: "inanimate",
                infoNotificationDelay: 60,
                reminderNotificationDelay: -1,
                delayedNotificationDelay: 300,
                delayedCounterDelay: 7200,
                confirmationNotificationDelay: 86400,
                confirmationNotificationRepetitions: 1,
                confirmationNotificationRepetitionsInterval: 3600,
                confirmationCounterDelay: 7200
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
        'booking.v1.resource.add',
        [
            'fields' => [
                'name' => 'Name',
                'description' => 'Description',
                'typeId' => 1,
                'isMain' => 'N',
                'isInfoNotificationOn' => 'Y',
                'templateTypeInfo' => 'inanimate',
                'isConfirmationNotificationOn' => 'Y',
                'templateTypeConfirmation' => 'animate',
                'isReminderNotificationOn' => 'N',
                'templateTypeReminder' => 'base',
                'isFeedbackNotificationOn' => 'Y',
                'templateTypeFeedback' => 'inanimate',
                'isDelayedNotificationOn' => 'Y',
                'templateTypeDelayed' => 'inanimate',
                'infoNotificationDelay' => 60,
                'reminderNotificationDelay' => -1,
                'delayedNotificationDelay' => 300,
                'delayedCounterDelay' => 7200,
                'confirmationNotificationDelay' => 86400,
                'confirmationNotificationRepetitions' => 1,
                'confirmationNotificationRepetitionsInterval' => 3600,
                'confirmationCounterDelay' => 7200
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
    res, err := client.Core().Call(ctx, "booking.v1.resource.add", b24.Params{
    	"fields": b24.Params{
    		"name":                            "Name",
    		"description":                     "Description",
    		"typeId":                          1,
    		"isMain":                          "N",
    		"isInfoNotificationOn":            "Y",
    		"templateTypeInfo":                "inanimate",
    		"isConfirmationNotificationOn":    "Y",
    		"templateTypeConfirmation":        "animate",
    		"isReminderNotificationOn":        "N",
    		"templateTypeReminder":            "base",
    		"isFeedbackNotificationOn":        "Y",
    		"templateTypeFeedback":            "inanimate",
    		"isDelayedNotificationOn":         "Y",
    		"templateTypeDelayed":             "inanimate",
    		"infoNotificationDelay":                       60,
    		"reminderNotificationDelay":                   -1,
    		"delayedNotificationDelay":                    300,
    		"delayedCounterDelay":             7200,
    		"confirmationNotificationDelay":               86400,
    		"confirmationNotificationRepetitions":         1,
    		"confirmationNotificationRepetitionsInterval": 3600,
    		"confirmationCounterDelay":        7200,
    	},
    })
    if err != nil {
    	return fmt.Errorf("booking.v1.resource.add: %w", err)
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
    "result": 17,
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
[`integer`](../../data-types.md) | Identifier of the created resource ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "1013",
    "error_description": "Resource type with id 17 does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Required fields: name` | The required parameter inside `fields` is missing ||
|| `100` | `Could not find value for parameter {fields}` | The required parameter `fields` is missing ||
|| `1006` | `ResourceType not found` | `typeId` is not specified ||
|| `1006` | `Failed creating new resource` | The resource could not be saved ||
|| `1013` | `Resource type with id {id} does not exist` | A non-existent `typeId` is specified ||
|| `422` | `Invalid value of the {field} field` | An invalid value of an enumerated field, for example `templateTypeInfo` ||
|| `0` | `Feature is not available` | The Booking tool is not available on the current plan ||
|| `0` | `Booking tool is disabled. Please contact your administrator.` | The Booking tool is disabled in the Bitrix24 settings ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./booking-v1-resource-update.md)
- [{#T}](./booking-v1-resource-get.md)
- [{#T}](./booking-v1-resource-list.md)
- [{#T}](./booking-v1-resource-delete.md)
- [{#T}](./resource-type/index.md)
- [{#T}](./slots/index.md)
- [{#T}](./events/on-booking-resource-add.md)
