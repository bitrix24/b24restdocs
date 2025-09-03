# Set Network Address Ranges timeman.networkrange.set

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `timeman.networkrange.set` sets the network address ranges for the office network.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **RANGES***
[`string`](../../data-types.md) | Network address ranges in the form of a list of objects. Each object contains a description of the [network address range](#ip_range).

```php
ranges: [
    {
        "ip_range": "10.0.0.0-10.255.255.255",
        "name": "Office Network 10.x.x.x"
    },
    {
        "ip_range": "10.10.0.1",
        "name": "Address 10.10.0.1"
    },
    ...
]
```

The range can contain:
- a block of addresses, for example, `10.0.0.0-10.255.255.255`
- a single address, for example, `10.10.0.1` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ranges":[{"ip_range":"10.0.0.0-10.255.255.255","name":"Office Network 10.x.x.x"},{"ip_range":"172.16.0.0-172.31.255.255","name":"Office Network 172.x.x.x"},{"ip_range":"192.168.0.0-192.168.255.255","name":"Office Network 192.168.x.x"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/timeman.networkrange.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ranges":[{"ip_range":"10.0.0.0-10.255.255.255","name":"Office Network 10.x.x.x"},{"ip_range":"172.16.0.0-172.31.255.255","name":"Office Network 172.x.x.x"},{"ip_range":"192.168.0.0-192.168.255.255","name":"Office Network 192.168.x.x"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.networkrange.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'timeman.networkrange.set',
    		{
    			ranges: [
    				{
    					"ip_range": "10.0.0.0-10.255.255.255",
    					"name": "Office Network 10.x.x.x"
    				},
    				{
    					"ip_range": "172.16.0.0-172.31.255.255",
    					"name": "Office Network 172.x.x.x"
    				},
    				{
    					"ip_range": "192.168.0.0-192.168.255.255",
    					"name": "Office Network 192.168.x.x"
    				}
    			]
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error.ex);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.networkrange.set',
                [
                    'ranges' => [
                        [
                            "ip_range" => "10.0.0.0-10.255.255.255",
                            "name"     => "Office Network 10.x.x.x"
                        ],
                        [
                            "ip_range" => "172.16.0.0-172.31.255.255",
                            "name"     => "Office Network 172.x.x.x"
                        ],
                        [
                            "ip_range" => "192.168.0.0-192.168.255.255",
                            "name"     => "Office Network 192.168.x.x"
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting network ranges: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'timeman.networkrange.set',
        {
            ranges: [
                {
                    "ip_range": "10.0.0.0-10.255.255.255",
                    "name": "Office Network 10.x.x.x"
                },
                {
                    "ip_range": "172.16.0.0-172.31.255.255",
                    "name": "Office Network 172.x.x.x"
                },
                {
                    "ip_range": "192.168.0.0-192.168.255.255",
                    "name": "Office Network 192.168.x.x"
                }
            ]
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.networkrange.set',
        [
            'ranges' => [
                [
                    'ip_range' => '10.0.0.0-10.255.255.255',
                    'name' => 'Office Network 10.x.x.x'
                ],
                [
                    'ip_range' => '172.16.0.0-172.31.255.255',
                    'name' => 'Office Network 172.x.x.x'
                ],
                [
                    'ip_range' => '192.168.0.0-192.168.255.255',
                    'name' => 'Office Network 192.168.x.x'
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
    "result": {
        "result": true
    },
    "time": {
        "start": 1742999863.0244141,
        "finish": 1742999863.057137,
        "duration": 0.03272294998168945,
        "processing": 0.0023658275604248047,
        "date_start": "2025-03-26T17:37:43+02:00",
        "date_finish": "2025-03-26T17:37:43+02:00",
        "operating_reset_at": 1743000463,
        "operating": 0
    }
}
```

### In case of range parsing error

HTTP Status: **200**

```json
{
    "result": {
        "result": false,
        "error_ranges": [
            {
                "ip_range": "a172.16.0.0-172.31.255.255",
                "name": "Office Network 172.x.x.x"
            }
        ]
    },
    "time": {
        "start": 1743058773.526674,
        "finish": 1743058773.5625639,
        "duration": 0.035889863967895508,
        "processing": 0.0005609989166259766,
        "date_start": "2025-03-27T09:59:33+02:00",
        "date_finish": "2025-03-27T09:59:33+02:00",
        "operating_reset_at": 1743059373,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response. Can have values:
- `true` — all ranges were successfully set
- `false` — there are ranges with errors ||
|| **error_range**
 [`array`](../../data-types.md) | Array of [ranges](#ip_range) where errors were found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Range Object {#ip_range}

#|
|| **Name**
`type` | **Description** ||
|| **ip_range**
 [`string`](../../data-types.md) | Network address range ||
|| **name**
 [`string`](../../data-types.md) | Name of the range ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "INVALID_FORMAT",
    "error_description": "A wrong format for the RANGES field is passed"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_ERROR` | You don't have access to use this method | The method is only available to the administrator ||
|| `INVALID_FORMAT` | A wrong format for the RANGES field is passed | An incorrect format was passed in the `RANGES` parameter ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-networkrange-get.md)
- [{#T}](./timeman-networkrange-check.md)