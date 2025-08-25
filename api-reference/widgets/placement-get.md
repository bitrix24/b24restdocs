# Get a list of registered widget placement handlers placement.get

> Scope: [`placement`, `depending on the placement`](../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of registered widget placement handlers.

## Method Parameters

The method has no parameters.

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/placement.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/placement.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"placement.get",
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'placement.get',
                []
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
        echo 'Error getting placements: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
 	BX24.callMethod(
        "placement.get",
        {},
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
        'placement.get',
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
    "result": [
        {
            "placement": "CRM_DEAL_LIST_TOOLBAR",
            "userId": 0,
            "handler": "https://myapp.com/?handler=1",
            "options": [],
            "title": "Add invoice",
            "description": "",
            "langAll": {
                "de": {
                    "TITLE": "Add invoice",
                    "DESCRIPTION": "",
                    "GROUP_NAME": "Documents"
                }
            }
        },
        {
            "placement": "CRM_DEAL_LIST_TOOLBAR",
            "userId": 0,
            "handler": "https://myapp.com/?handler=1",
            "options": [],
            "title": "Import invoice",
            "description": "",
            "langAll": {
                "de": {
                    "TITLE": "Import invoice",
                    "DESCRIPTION": "",
                    "GROUP_NAME": "Documents"
                }
            }
        },
        {
            "placement": "IM_CONTEXT_MENU",
            "userId": 0,
            "handler": "https://myapp.com/?handler=2",
            "options": {
                "extranet": "N",
                "context": "ALL",
                "role": "USER"
            },
            "title": "My App 1",
            "description": "",
            "langAll": {
                "de": {
                    "TITLE": "My App 1",
                    "DESCRIPTION": "",
                    "GROUP_NAME": ""
                }
            }
        },
        {
            "placement": "PAGE_BACKGROUND_WORKER",
            "userId": 1,
            "handler": "https://myapp.com/?handler=3",
            "options": {
                "errorHandlerUrl": "https://myapp.com/?handler=3"
            },
            "title": "My App 2",
            "description": "",
            "langAll": {
                "de": {
                    "TITLE": "My App 2",
                    "DESCRIPTION": "",
                    "GROUP_NAME": ""
                }
            }
        }
    ],
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Returns a list of registered widget handlers. The structure of each element corresponds to the parameters of the [handler registration method](./placement-bind.md#params)

||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**, **403**, **200**

```json
{
    "error": "INVALID_REQUEST",
    "error_description": "Https required"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./placements.md)
- [{#T}](./placement-list.md)
- [{#T}](./placement-bind.md)
- [{#T}](./placement-unbind.md)
- [{#T}](./ui-interaction/index.md)