# Set Connections for Booking booking.v1.booking.externalData.set

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.externalData.set` establishes connections for the specified booking.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **bookingId***
[`integer`](../../../data-types.md) | Booking identifier.
Can be obtained using the methods [booking.v1.booking.add](../booking-v1-booking-add.md) and [booking.v1.booking.list](../booking-v1-booking-list.md) ||
|| **externalData***
[`array`](../../../data-types.md) | Array of objects containing items for binding [(detailed description)](#externalData) ||
|#

### Parameter externalData {#externalData}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../../../data-types.md) | Module identifier, for example `crm` ||
|| **entityTypeId***
[`string`](../../../data-types.md) | Object type ID, for example `DEAL` ||
|| **value***
[`string`](../../../data-types.md) | Item ID, for example `1` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"bookingId":14,"externalData":[{"moduleId":"crm","entityTypeId":"DEAL","value":"1"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.booking.externalData.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"bookingId":14,"externalData":[{"moduleId":"crm","entityTypeId":"DEAL","value":"1"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.booking.externalData.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"booking.v1.booking.externalData.set",
    		{
    			bookingId: 14,
    			externalData: [
    				{
    					moduleId: "crm",
    					entityTypeId: "DEAL",
    					value: "1"
    				}
    			]
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
                'booking.v1.booking.externalData.set',
                [
                    'bookingId'    => 14,
                    'externalData' => [
                        [
                            'moduleId'     => 'crm',
                            'entityTypeId' => 'DEAL',
                            'value'        => '1',
                        ],
                    ],
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
        echo 'Error setting external data: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.booking.externalData.set",
        {
            bookingId: 14,
            externalData: [
                {
                    moduleId: "crm",
                    entityTypeId: "DEAL",
                    value: "1"
                }
            ]
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
        'booking.v1.booking.externalData.set',
        [
            'bookingId' => 14,
            'externalData' => [
                [
                    'moduleId' => 'crm',
                    'entityTypeId' => 'DEAL',
                    'value' => '1'
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
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 1021,
    "error_description": "Booking not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Required fields:` | Required parameter not provided within `externalData` ||
|| `1021` | `Booking not found` | Booking with the specified `id` not found ||
|| `100` | `Could not find value for parameter` | Required parameter not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-booking-externaldata-unset.md)
- [{#T}](./booking-v1-booking-externaldata-list.md)