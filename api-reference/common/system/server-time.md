# Get Current Server Time server.time

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `server.time` returns the current server time in the format `YYYY-MM-DDThh:mm:ss±hh:mm`.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/server.time
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/server.time
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"server.time",
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
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
                'server.time',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Server time: ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting server time: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "server.time",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'server.time',
        []
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
    {
        "result": "2024-08-05T09:02:13+00:00",
        "time": {
            "start": 1722848533.40768,
            "finish": 1722848533.44277,
            "duration": 0.0350871086120606,
            "processing": 0.000040054321289062,
            "date_start": "2024-08-05T09:02:13+00:00",
            "date_finish": "2024-08-05T09:02:13+00:00",
            "operating": 0
        }
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../data-types.md) | Server time in the format `YYYY-MM-DDThh:mm:ss±hh:mm` ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./method-get.md)
- [{#T}](./scope.md)
- [{#T}](./app-info.md)
- [{#T}](./access-name.md)
- [{#T}](./feature-get.md)
- [{#T}](./methods.md)