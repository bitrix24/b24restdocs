# Enable Debug Mode landing.repowidget.debug

> Scope: [`landing`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.repowidget.debug` enables debug mode for all widgets of the current application. In this case, the vue application will report more errors in the js console for developer convenience. By default, the mode is disabled.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **enable**
[`boolean`](../data-types.md) | Debug mode activation flag ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.repowidget.debug', {
    			enable: true,
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.info(result);
    }
    catch(error)
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
                'landing.repowidget.debug',
                [
                    'enable' => true,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling landing repowidget debug: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repowidget.debug', {
            enable: true,
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.repowidget.debug',
        [
            'enable' => true,
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
        "start": 1713949410.036288,
        "finish": 1713949411.632775,
        "duration": 1.596487045288086,
        "processing": 0.6458539962768555,
        "date_start": "2024-04-24T11:03:30+02:00",
        "date_finish": "2024-04-24T11:03:31+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | On success — `true`, otherwise — error ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"APP_NOT_FOUND",
    "error_description":"Cannot find REST application"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `APP_NOT_FOUND` | REST application not found. Occurs when there are issues with the authorization of the REST application on the account ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-repowidget-register.md)
- [{#T}](./landing-repowidget-unregister.md)
- [{#T}](./landing-repowidget-get-list.md)