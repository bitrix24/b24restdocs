# Update Booking booking.v1.booking.update

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.update` updates the booking information.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Booking identifier ||
|| **fields***
[`object`](../../data-types.md) | Object containing field values for updating the booking [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **resourceIds**
[`array`](../../data-types.md#array) | Array of resource identifiers. 
Resource IDs can be obtained using the method [booking.v1.resource.list](../resource/booking-v1-resource-list.md) ||
|| **name**
[`string`](../../data-types.md) | Booking name ||
|| **description**
[`string`](../../data-types.md) | Booking description ||
|| **datePeriod**
[`object`](../../data-types.md#object) | Object containing the booking time [(detailed description)](#datePeriod) ||
|#

### Parameter datePeriod {#datePeriod}

#|
|| **Name**
`type` | **Description** ||
|| **from***
[`object`](../../data-types.md#object) | Start time of the booking in the format `{"timestamp": "1723446900", "timezone": "Europe/Berlin"}` ||
|| **to***
[`object`](../../data-types.md#object) | End time of the booking in the format `{"timestamp": "1723447800", "timezone": "Europe/Berlin"}` ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":10,"fields":{"name":"Name","description":"Description","resourceIds":[1,2,3],"datePeriod":{"from":{"timestamp":1723446900,"timezone":"Europe/Berlin"},"to":{"timestamp":1723447800,"timezone":"Europe/Berlin"}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.booking.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":10,"fields":{"name":"Name","description":"Description","resourceIds":[1,2,3],"datePeriod":{"from":{"timestamp":1723446900,"timezone":"Europe/Berlin"},"to":{"timestamp":1723447800,"timezone":"Europe/Berlin"}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.booking.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'booking.v1.booking.update',
    		{
    			id: 10,
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
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
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
                'booking.v1.booking.update',
                [
                    'id' => 10,
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
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
            // Your required data processing logic
            processData($result->data());
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating booking: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.booking.update",
        {
            id: 10,
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
        'booking.v1.booking.update',
        [
            'id' => 10,
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

HTTP Status: **200**

```json
{
    "result": true,
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
[`boolean`](../../data-types.md) | Root element of the response, contains `true` on success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `1019` | `Booking not found` | Entry with such `id` not found ||
|| `1018` | `Empty resource collection` | Resources unavailable for the specified time period, empty resource array or such resources do not exist ||
|| `0` | `Required fields:` | Required parameter not passed within `fields` ||
|| `100` | `Could not find value for parameter ` | Required parameter not passed ||
|| `422` | `Invalid date period` | Incorrect time period ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-booking-createfromwaitlist.md)
- [{#T}](./booking-v1-booking-delete.md)
- [{#T}](./booking-v1-booking-add.md)
- [{#T}](./booking-v1-booking-list.md)
- [{#T}](./booking-v1-booking-get.md)