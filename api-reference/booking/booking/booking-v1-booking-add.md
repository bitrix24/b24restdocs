# Add Booking booking.v1.booking.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.add` adds a new booking for a resource.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object containing field values for creating a booking [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **resourceIds***
[`array`](../../data-types.md#array) | An array of resource IDs for the booking. 
Resource IDs can be obtained using the method [booking.v1.resource.list](../resource/booking-v1-resource-list.md) ||
|| **name**
[`string`](../../data-types.md) | The name of the booking. 
Default value is an empty string ||
|| **description**
[`string`](../../data-types.md) | The description of the booking. 
Default value is an empty string ||
|| **datePeriod***
[`object`](../../data-types.md#object) | An object containing the booking time [(detailed description)](#datePeriod) ||
|#

### Parameter datePeriod {#datePeriod}

#|
|| **Name**
`type` | **Description** ||
|| **from***
[`object`](../../data-types.md#object) | The start time of the booking in the format `{"timestamp": "1723446900", "timezone": "Europe/Berlin"}`||
|| **to***
[`object`](../../data-types.md#object) | The end time of the booking in the format `{"timestamp": "1723447800", "timezone": "Europe/Berlin"}` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Name","description":"Description","resourceIds":[1,2,3],"datePeriod":{"from":{"timestamp":1723446900,"timezone":"Europe/Berlin"},"to":{"timestamp":1723447800,"timezone":"Europe/Berlin"}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.booking.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Name","description":"Description","resourceIds":[1,2,3],"datePeriod":{"from":{"timestamp":1723446900,"timezone":"Europe/Berlin"},"to":{"timestamp":1723447800,"timezone":"Europe/Berlin"}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.booking.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AddBookingResult = {
      id: number
    }

    try {
      const response = await $b24.actions.v2.call.make<AddBookingResult>({
        method: 'booking.v1.booking.add',
        params: {
          fields: {
            name: 'Name',
            description: 'Description',
            resourceIds: [1, 2, 3],
            datePeriod: {
              from: {
                timestamp: 1723446900,
                timezone: 'Europe/Berlin',
              },
              to: {
                timestamp: 1723447800,
                timezone: 'Europe/Berlin',
              },
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added booking ID:', result.id)
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
      async function addBooking() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'booking.v1.booking.add',
            params: {
              fields: {
                name: 'Name',
                description: 'Description',
                resourceIds: [1, 2, 3],
                datePeriod: {
                  from: {
                    timestamp: 1723446900,
                    timezone: 'Europe/Berlin',
                  },
                  to: {
                    timestamp: 1723447800,
                    timezone: 'Europe/Berlin',
                  },
                },
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
          console.info('Added booking ID:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addBooking)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'booking.v1.booking.add',
                [
                    'fields' => [
                        'name'        => 'Name',
                        'description' => 'Description',
                        'resourceIds' => [1, 2, 3],
                        'datePeriod'  => [
                            'from' => [
                                'timestamp' => 1723446900,
                                'timezone'  => 'Europe/Berlin',
                            ],
                            'to'   => [
                                'timestamp' => 1723447800,
                                'timezone'  => 'Europe/Berlin',
                            ],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding booking: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.booking.add",
        {
            fields: {
                name: "Name",
                description: "Description",
                resourceIds: [1, 2, 3],
                datePeriod: {
                    from: {
                        timestamp: 1723446900,
                        timezone: "Europe/Berlin"
                    },
                    to: {
                        timestamp: 1723447800,
                        timezone: "Europe/Berlin"
                    }
                }
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
        'booking.v1.booking.add',
        [
            'fields' => [
                'name' => 'Name',
                'description' => 'Description',
                'resourceIds' => [1, 2, 3],
                'datePeriod' => [
                    'from' => [
                        'timestamp' => 1723446900,
                        'timezone' => 'Europe/Berlin'
                    ],
                    'to' => [
                        'timestamp' => 1723447800,
                        'timezone' => 'Europe/Berlin'
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 1823
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
[`integer`](../../data-types.md) | The root element of the response, contains the ID of the added booking ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 422,
    "error_description": "Invalid date period"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `1018` | `Empty resource collection` | Resources are unavailable for the specified time period, empty array of resources or such resources do not exist ||
|| `0` | `Required fields:` | A required parameter inside `fields` was not provided ||
|| `100` | `Could not find value for parameter ` | A required parameter was not provided ||
|| `422` | `Invalid date period` | Incorrect time period ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-booking-createfromwaitlist.md)
- [{#T}](./booking-v1-booking-update.md)
- [{#T}](./booking-v1-booking-get.md)
- [{#T}](./booking-v1-booking-list.md)
- [{#T}](./booking-v1-booking-delete.md)