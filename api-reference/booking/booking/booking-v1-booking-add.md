# Add Booking booking.v1.booking.add

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

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'booking.v1.booking.add',
    		{
    			fields: {
    				name: 'Name',
    				description: 'Description',
    				resourceIds: [1, 2, 3],
    				datePeriod: {
    					from: {
    						timestamp: 1723446900,
    						timezone: 'Europe/Berlin'
    					},
    					to: {
    						timestamp: 1723447800,
    						timezone: 'Europe/Berlin'
    					}
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    		console.error(result.error());
    	else
    		console.dir(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
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