# Set a New Chat Name imconnector.chat.name.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.chat.name.set` renames the Open Channel chat in Bitrix24 that corresponds to the external system dialog with the specified `CHAT_ID`. In the external system, the dialog name stays the same.

Bitrix24 looks for an open session by the combination of `CONNECTOR`, `LINE`, and `CHAT_ID`. If there is no such session, the method returns the `CHAT_RENAMING_FAILED` error.

For connectors with `CHAT_GROUP: false`, `USER_ID` also takes part in the search, but it behaves differently: if no interlocutor with this identifier is found, the method returns `SUCCESS: true` and does not rename the chat.

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
[`integer`](../../data-types.md) | Identifier of the Open Channel.

The identifier can be obtained using the methods [imopenlines.config.get](../openlines/imopenlines-config-get.md) and [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) ||
|| **CHAT_ID***  
[`string`](../../data-types.md) | Identifier of the chat in the external system — the same value that is passed in `chat.id` of the [imconnector.send.messages](./imconnector-send-messages.md) method ||
|| **NAME***  
[`string`](../../data-types.md) | New name for the chat ||
|| **USER_ID**  
[`string`](../../data-types.md) | Identifier of the interlocutor in the external system — the same value that is passed in `user.id` of the [imconnector.send.messages](./imconnector-send-messages.md) method. A Bitrix24 user identifier does not work here.

The parameter is required for connectors registered with the `CHAT_GROUP` parameter set to `false` in the [imconnector.register](./imconnector-register.md) method. If `CHAT_GROUP` is `true`, the value passed is ignored ||
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ChatNameSetResult = {
      SUCCESS: boolean
      DATA: {
        RESULT: Record<string, unknown>
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ChatNameSetResult>({
        method: 'imconnector.chat.name.set',
        params: {
          CONNECTOR: 'connector',
          LINE: 105,
          CHAT_ID: '47e007b1-ee15-43db-bcba-1c26e5884d3f',
          NAME: 'New dialog name',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('SUCCESS:', result.SUCCESS, 'DATA:', result.DATA)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function setChatName() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imconnector.chat.name.set',
            params: {
              CONNECTOR: 'connector',
              LINE: 105,
              CHAT_ID: '47e007b1-ee15-43db-bcba-1c26e5884d3f',
              NAME: 'New dialog name',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('SUCCESS:', result.SUCCESS, 'DATA:', result.DATA)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setChatName)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imconnector.chat.name.set(
            connector="connector",
            line=105,
            chat_id="47e007b1-ee15-43db-bcba-1c26e5884d3f",
            name="New dialog name",
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imconnector.chat.name.set", b24.Params{
    	"CONNECTOR": "connector",
    	"LINE":      105,
    	"CHAT_ID":   "47e007b1-ee15-43db-bcba-1c26e5884d3f",
    	"NAME":      "New Dialog Name",
    })
    if err != nil {
    	return fmt.Errorf("imconnector.chat.name.set: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
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
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Result of setting the chat name [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Returns `true` if the request is accepted and passed to the connector handler.

The method never returns `false`: rejections come as an error with the HTTP status `400`. The only exception is an unrecognized `USER_ID`, described below in the "Error Handling" section ||
|| **DATA**
[`object`](../../data-types.md) | Service object. The `RESULT` field is reserved for the connector handler result and always comes as an empty object `{}` in the response ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "NOT_ACTIVE_LINE",
    "error_description": "The line with this ID is inactive or does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | The method was called outside the application OAuth context ||
|| `400` | `ERROR_ARGUMENT` | Argument 'NAME' is null or empty | One of the parameters is missing or empty: `CONNECTOR`, `LINE`, `CHAT_ID`, `NAME`, or the conditionally required `USER_ID`. The parameter name comes in the message and in a separate `argument` field ||
|| `400` | `NOT_ACTIVE_LINE` | The line with this ID is inactive or does not exist | The connector is not enabled on this line, or there is no line with this `LINE`. Enable the connector using the [imconnector.activate](./imconnector-activate.md) method ||
|| `400` | `IMCONNECTOR_NO_CORRECT_PROVIDER` | Unable to find a suitable provider for the connector | A connector with this code is not registered in Bitrix24 ||
|| `400` | `CHAT_RENAMING_FAILED` | Chat renaming failed | There is no open session for the combination of parameters passed, or the chat could not be renamed ||
|#

Not every rejection comes as an error: if the interlocutor in the external system could not be identified by `USER_ID`, the method returns `SUCCESS: true`, but the chat is not renamed.

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
- [{#T}](../../../tutorials/openlines/example-connector.md)
