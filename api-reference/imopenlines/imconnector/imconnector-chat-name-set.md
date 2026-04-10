# Set a New Chat Name imconnector.chat.name.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imconnector`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.chat.name.set` sets a new name for the chat.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %} 

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR***  
[`string`](../../data-types.md) | The string code of the connector specified in the `ID` parameter when calling [imconnector.register](./imconnector-register.md) ||
|| **LINE***  
[`integer`](../../data-types.md) | Identifier of the Open Channel

The identifier can be obtained using the methods [imopenlines.config.get](../openlines/imopenlines-config-get.md) and [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) ||
|| **CHAT_ID***  
[`string`](../../data-types.md) | Identifier of the chat in the external system ||
|| **NAME***  
[`string`](../../data-types.md) | New name for the chat ||
|| **USER_ID**  
[`string`](../../data-types.md) | User identifier. This parameter is mandatory only for connectors without group chats from the external side. For such a connector, the `CHAT_GROUP` parameter in the [imconnector.register](./imconnector-register.md) method must be set to `N` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CONNECTOR":"connector","LINE":105,"CHAT_ID":"47e007b1-ee15-43db-bcba-1c26e5884d3f","NAME":"New Dialog Name","auth":"**put_access_token_here**"}' \
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
                LINE: 105,
                CHAT_ID: '47e007b1-ee15-43db-bcba-1c26e5884d3f',
                NAME: 'New Dialog Name'
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
            'LINE'      => 105,
            'CHAT_ID'   => '47e007b1-ee15-43db-bcba-1c26e5884d3f',
            'NAME'      => 'New Dialog Name',
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
        LINE: 105,
        CHAT_ID: '47e007b1-ee15-43db-bcba-1c26e5884d3f',
        NAME: 'New Dialog Name'
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
        'LINE' => 105,
        'CHAT_ID' => '47e007b1-ee15-43db-bcba-1c26e5884d3f',
        'NAME' => 'New Dialog Name'
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

HTTP Status: **200**

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

HTTP Status: **400**

```json
{
    "error": "NOT_ACTIVE_LINE",
    "error_description": "The line with this ID is inactive or does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** ||
|| `400` | `NOT_ACTIVE_LINE` | The line with this ID is inactive or does not exist ||
|| `400` | `IMCONNECTOR_NO_CORRECT_PROVIDER` | Unable to find a suitable provider for the connector ||
|| `400` | `ERROR_ARGUMENT` | Required parameters `NAME`, `CHAT_ID`, `USER_ID`, `CONNECTOR`, or `LINE` are missing ||
|| `400` | `CHAT_RENAMING_FAILED` | Failed to rename the chat ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

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
- [{#T}](./imconnector-chat-name-set.md)