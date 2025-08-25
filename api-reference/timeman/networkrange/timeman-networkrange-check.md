# Check IP Address timeman.networkrange.check

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `timeman.networkrange.check` checks if an IP address falls within the ranges of the office network.

For the method to work correctly, at least one range must be set.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **IP**
[`string`](../../data-types.md) | The IP address to check. For example, `10.10.255.25`.

If not specified, the check will be performed for the current IP address ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IP":"10.10.255.255"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.networkrange.check
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IP":"10.10.255.255","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.networkrange.check
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'timeman.networkrange.check',
    		{
    			'IP': '10.10.255.255'
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
                'timeman.networkrange.check',
                [
                    'IP' => '10.10.255.255'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error()->ex);
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error checking network range: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'timeman.networkrange.check',
        {
            'IP': '10.10.255.255'
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
        'timeman.networkrange.check',
        [
            'IP' => '10.10.255.255'
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
        "ip": "10.10.255.255",
        "range": "10.0.0.0-10.255.255.255",
        "name": "10.x.x.x"
    },
    "time": {
        "start": 1742997578.1675889,
        "finish": 1742997578.205729,
        "duration": 0.038140058517456055,
        "processing": 0.0028028488159179688,
        "date_start": "2025-03-26T16:59:38+02:00",
        "date_finish": "2025-03-26T16:59:38+02:00",
        "operating_reset_at": 1742998178,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response.

Contains an object describing the range that the IP address falls into.

Will return `false` if the IP address does not fall into any office network range ||
|| **ip**
 [`string`](../../data-types.md) | The IP address that was checked ||
|| **range**
 [`string`](../../data-types.md) | The range that the IP address falls into ||
|| **name**
 [`string`](../../data-types.md) | The name of the range that the IP address falls into ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You don't have access to use this method"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_ERROR` | You don't have access to use this method | The method is only available to administrators ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-networkrange-get.md)
- [{#T}](./timeman-networkrange-set.md)