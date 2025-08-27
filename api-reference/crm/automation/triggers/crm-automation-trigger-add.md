# Add Trigger crm.automation.trigger.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator with access to CRM in the application context 

This method adds a trigger.

The method can only be executed in the application context, as the added triggers are tied to this application. 

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../../data-types.md) | Internal unique (within the application) identifier of the trigger. Must match the pattern `[a-z0-9\.\-_]`.

If an existing trigger identifier `CODE` is provided, the trigger name `NAME` will be updated. ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the trigger ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"c5u4m","NAME":"trigger name"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automation.trigger.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"c5u4m","NAME":"trigger name","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.automation.trigger.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.automation.trigger.add',
    		{
    			"CODE": 'c5u4m',
    			"NAME": 'trigger name'
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
                'crm.automation.trigger.add',
                [
                    'CODE' => 'c5u4m',
                    'NAME' => 'trigger name',
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
        echo 'Error adding automation trigger: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.automation.trigger.add',
        {
            "CODE": 'c5u4m',
            "NAME": 'trigger name'
        },
        function(result) 
        {
            if(result.error())
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
        'crm.automation.trigger.add',
        [
            'CODE' => 'c5u4m',
            'NAME' => 'trigger name'
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
    "result": true,
    "time": {
        "start":1718884406.366687,
        "finish":1718884406.80718,
        "duration":0.4404928684234619,
        "processing":0.03356289863586426,
        "date_start":"2024-06-20T11:53:26+00:00",
        "date_finish":"2024-06-20T11:53:26+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns `true` if the trigger was successfully added ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ACCESS_DENIED",
    "error_description":"Access denied! Application context required"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | Access denied. | User did not pass the preliminary access rights check for CRM ||
|| ACCESS_DENIED | Access denied! Admin permissions required | Admin rights check failed ||
|| ACCESS_DENIED | Access denied! Application context required | Method called outside of application context ||
|| Empty string | Empty trigger code! | Empty parameter `CODE` ||
|| Empty string | Wrong trigger code! | Parameter `CODE` does not match the pattern `[a-z0-9\.\-_]` ||
|| Empty string | Empty trigger name! | Empty parameter `NAME` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-automation-trigger-execute.md)
- [{#T}](./crm-automation-trigger-list.md)
- [{#T}](./crm-automation-trigger-delete.md)