# Set Connector Settings imconnector.connector.data.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.connector.data.set` retains in Bitrix24 the configurations of a custom connector on the specified open line: the external channel identifier, the links to it, and the name.

The configurations appear in the operator interface only after the connector is enabled on this line: the [imconnector.status](./imconnector-status.md) method must return `STATUS: true`.

When the connector is disabled by calling [imconnector.activate](./imconnector-activate.md) with the `ACTIVE: 0` parameter, the retained configurations are deleted along with the status record.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR***
[`string`](../../data-types.md) | String code of the connector specified in the `ID` parameter when calling [imconnector.register](./imconnector-register.md) ||
|| **LINE***
[`integer`](../../data-types.md) | Identifier of the open line.

The identifier can be obtained using the methods [imopenlines.config.get](../openlines/imopenlines-config-get.md) and [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) ||
|| **DATA***
[`object`](../../data-types.md) | Object containing the channel settings in the external system.

The structure of the object is described in detail [below](#data) ||
|#

### DATA Parameter {#data}

All fields of the object are optional. The method does not overwrite the settings as a whole: the fields you pass are updated, and the fields you omit keep their previous values.

An empty string is ignored, so the method cannot clear a value that is already retained.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Identifier of the channel in the external system, for example, `channel-123`.

The identifier should be taken from the external system. If none is available, use a unique key, such as a combination of the connector code and the line identifier: `myconnector_line107` ||
|| **URL**
[`string`](../../data-types.md) | Full link to the chat or channel in the external system ||
|| **URL_IM**
[`string`](../../data-types.md) | Link to the chat that the operator opens from the Open Channels interface. If there is no separate link for the operator, you can omit the field — the connector keeps only `URL` ||
|| **NAME**
[`string`](../../data-types.md) | Name of the channel that the operator sees ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CONNECTOR":"myconnector",
        "LINE":107,
        "DATA":{
          "ID":"channel-123",
          "URL":"https://example.com/chats/123",
          "NAME":"Support Channel"
        },
        "auth":"**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imconnector.connector.data.set
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'imconnector.connector.data.set',
        params: {
          CONNECTOR: 'myconnector',
          LINE: 107,
          DATA: {
            ID: 'channel-123',
            URL: 'https://example.com/chats/123',
            NAME: 'Support Channel',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Connector data saved:', result)
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
      async function setConnectorData() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imconnector.connector.data.set',
            params: {
              CONNECTOR: 'myconnector',
              LINE: 107,
              DATA: {
                ID: 'channel-123',
                URL: 'https://example.com/chats/123',
                NAME: 'Support Channel',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Connector data saved:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setConnectorData)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imconnector.connector.data.set(
            connector="myconnector",
            line=107,
            data={
                "ID": "channel-123",
                "URL": "https://example.com/chats/123",
                "NAME": "Support Channel",
            },
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
    $result = $b24Service->core->call(
        'imconnector.connector.data.set',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'DATA' => [
                'ID' => 'channel-123',
                'URL' => 'https://example.com/chats/123',
                'NAME' => 'Support Channel',
            ],
        ]
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'imconnector.connector.data.set',
      {
        CONNECTOR: 'myconnector',
        LINE: 107,
        DATA: {
          ID: 'channel-123',
          URL: 'https://example.com/chats/123',
          NAME: 'Support Channel',
        },
      },
      function(result) {
        console.log(result.data());
      }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'imconnector.connector.data.set',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'DATA' => [
                'ID' => 'channel-123',
                'URL' => 'https://example.com/chats/123',
                'NAME' => 'Support Channel',
            ],
        ]
    );
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imconnector.connector.data.set", b24.Params{
    	"CONNECTOR": "myconnector",
    	"LINE":      107,
    	"DATA": b24.Params{
    		"ID":   "channel-123",
    		"URL":  "https://example.com/chats/123",
    		"NAME": "Support Channel",
    	},
    })
    if err != nil {
    	return fmt.Errorf("imconnector.connector.data.set: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773267900.126,
        "finish": 1773267900.489,
        "duration": 0.3630001544952393,
        "processing": 0.0884850025177002,
        "date_start": "2026-03-11T14:25:00+03:00",
        "date_finish": "2026-03-11T14:25:00+03:00",
        "operating_reset_at": 1773268500,
        "operating": 0.0884850025177002
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Always `true` if the required parameters are provided: the method does not check whether the connector and the open line exist ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'DATA' is null or empty",
    "argument": "DATA"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | Method called outside the application context OAuth ||
|| `400` | `ERROR_ARGUMENT` | Argument 'CONNECTOR' is null or empty | `CONNECTOR` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'LINE' is null or empty | `LINE` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'DATA' is null or empty | The `DATA` key is not provided. An empty `DATA` object does not cause an error ||
|#

The body of the `ERROR_ARGUMENT` error contains an additional `argument` field with the parameter name — there is no need to parse the message text.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)
- [{#T}](../../../tutorials/openlines/example-connector.md)