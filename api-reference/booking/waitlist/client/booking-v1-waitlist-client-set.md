# Add Clients to the Waitlist Record in booking.v1.waitlist.client.set

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.waitlist.client.set` sets clients for the specified record in the waitlist.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **waitListId***
[`integer`](../../../data-types.md) | Identifier of the waitlist record. 
Can be obtained using the methods [booking.v1.waitlist.add](../booking-v1-waitlist-add.md) and [booking.v1.waitlist.list](../booking-v1-waitlist-list.md) ||
|| **clients***
[`array`](../../../data-types.md) | Array of objects containing information about clients [(detailed description)](#clients) ||
|#

### Clients Parameter {#clients}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Client identifier, can be obtained using the method [crm.item.list](../../../crm/universal/crm-item-list.md) for contacts and companies ||
|| **type***
[`object`](../../../data-types.md) | Client type in the format `{"module": "crm", "code": "CONTACT"}`.
Possible `code` values: 
- `CONTACT` — [CRM contact](../../../crm/contacts/index.md)
- `COMPANY` — [CRM company](../../../crm/companies/index.md)

The structure of the object is returned by the method [booking.v1.clienttype.list](../../booking-v1-clienttype-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":4,"clients":[{"id":1,"type":{"module":"crm","code":"CONTACT"}},{"id":2,"type":{"module":"crm","code":"CONTACT"}}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.waitlist.client.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":4,"clients":[{"id":1,"type":{"module":"crm","code":"CONTACT"}},{"id":2,"type":{"module":"crm","code":"CONTACT"}}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.waitlist.client.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"booking.v1.waitlist.client.set",
    		{
    			waitListId: 4,
    			clients: [
    				{
    					id: 1,
    					type: {
    						module: "crm",
    						code: "CONTACT"
    					}
    				},
    				{
    					id: 2,
    					type: {
    						module: "crm",
    						code: "CONTACT"
    					}
    				}
    			]
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'booking.v1.waitlist.client.set',
                [
                    'waitListId' => 4,
                    'clients'    => [
                        [
                            'id'   => 1,
                            'type' => [
                                'module' => 'crm',
                                'code'   => 'CONTACT',
                            ],
                        ],
                        [
                            'id'   => 2,
                            'type' => [
                                'module' => 'crm',
                                'code'   => 'CONTACT',
                            ],
                        ],
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
        echo 'Error setting waitlist clients: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.waitlist.client.set",
        {
            waitListId: 4,
            clients: [
                {
                    id: 1,
                    type: {
                        module: "crm",
                        code: "CONTACT"
                    }
                },
                {
                    id: 2,
                    type: {
                        module: "crm",
                        code: "CONTACT"
                    }
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
        'booking.v1.waitlist.client.set',
        [
            'waitListId' => 4,
            'clients' => [
                [
                    'id' => 1,
                    'type' => [
                        'module' => 'crm',
                        'code' => 'CONTACT'
                    ]
                ],
                [
                    'id' => 2,
                    'type' => [
                        'module' => 'crm',
                        'code' => 'CONTACT'
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
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 1040,
    "error_description": "Wait list not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Required fields:` | Required parameter not provided within `clients` ||
|| `1040` | `Wait list not found` | Waitlist with the specified `id` not found ||
|| `100` | `Could not find value for parameter` | Required parameter not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-waitlist-client-unset.md)
- [{#T}](./booking-v1-waitlist-client-list.md)