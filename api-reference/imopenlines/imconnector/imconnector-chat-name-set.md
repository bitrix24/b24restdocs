# Set New Chat Name imconnector.chat.name.set

> Scope: [`imconnector`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method sets a new chat name.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR***
[`string`](../../data-types.md) | Connector identifier ||
|| **LINE***
[`string`](../../data-types.md) | Open line identifier ||
|| **CHAT_ID***
[`string`](../../data-types.md) | Chat identifier in the external system ||
|| **NAME***
[`string`](../../data-types.md) | New chat name ||
|| **USER_ID**
[`string`](../../data-types.md) | User identifier. This parameter is required only for connectors without group chats from the external side. For such a connector, the parameter `CHAT_GROUP` in the method [imconnector.register](./imconnector-register.md) must be equal to `N` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CONNECTOR":"connector","LINE":"105","CHAT_ID":"47e007b1-ee15-43db-bcba-1c26e5884d3f","NAME":"New dialog name"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imconnector.chat.name.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CONNECTOR":"connector","LINE":"105","CHAT_ID":"47e007b1-ee15-43db-bcba-1c26e5884d3f","NAME":"New dialog name","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imconnector.chat.name.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'imconnector.chat.name.set',
    		{
    			CONNECTOR: 'connector',
    			LINE: '105',
    			CHAT_ID: '47e007b1-ee15-43db-bcba-1c26e5884d3f',
    			NAME: 'New dialog name'
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		alert("Error: " + result.error());
    	else
    		alert("Success: " + result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $params = [
            'CONNECTOR' => 'connector',
            'LINE'      => '105',
            'CHAT_ID'   => '47e007b1-ee15-43db-bcba-1c26e5884d3f',
            'NAME'      => 'New dialog name',
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'imconnector.chat.name.set',
                $params
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting chat name: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var params = {
        CONNECTOR: 'connector',
        LINE: '105',
        CHAT_ID: '47e007b1-ee15-43db-bcba-1c26e5884d3f',
        NAME: 'New dialog name'
    };
    BX24.callMethod(
        'imconnector.chat.name.set',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $params = [
        'CONNECTOR' => 'connector',
        'LINE' => '105',
        'CHAT_ID' => '47e007b1-ee15-43db-bcba-1c26e5884d3f',
        'NAME' => 'New dialog name'
    ];

    $result = CRest::call(
        'imconnector.chat.name.set',
        $params
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
    "answer": {
        "result": {
            "SUCCESS": true,
            "DATA": {
                "RESULT": {}
            }
        },
        "time": {
            "start": 1732110908.525962,
            "finish": 1732110908.879113,
            "duration": 0.3531508445739746,
            "processing": 0.07694888114929199,
            "date_start": "2024-11-20T15:55:08+02:00",
            "date_finish": "2024-11-20T15:55:08+02:00"
        }
    },
    "query": {
        "method": "imconnector.chat.name.set",
        "data": {
            "CONNECTOR": "newcustomconnector",
            "LINE": "105",
            "CHAT_ID": "1",
            "NAME": "name"
        }
    },
    "status": 200
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Returns `true` when the new chat name is successfully set ||
|| **DATA**
[`object`](../../data-types.md) | Contains the `RESULT` object with parameters of the new chat name ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_ACTIVE_LINE",
    "error_description": "The line with this ID is inactive or does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `NOT_ACTIVE_LINE` | The line with this ID is inactive or does not exist ||
|| `IMCONNECTOR_NO_CORRECT_PROVIDER` | Could not find a suitable provider for the connector ||
|| `ERROR_ARGUMENT` | Required parameters `NAME`, `CHAT_ID`, `USER_ID`, `CONNECTOR`, or `LINE` are missing ||
|| `CHAT_RENAMING_FAILED` | Failed to rename the chat ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tutorials.md)
- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-connector-data-set.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-send-status-reading.md)