# Activate Connector imconnector.activate

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.activate` enables or disables the connector on the specified open line.

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
[`integer`](../../data-types.md) | The identifier of the open line.

The identifier can be obtained using the methods [imopenlines.config.get](../openlines/imopenlines-config-get.md) and [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) ||
|| **ACTIVE***
[`string`](../../data-types.md) | Enabling flag. Bitrix24 distinguishes only between empty and non-empty values:

- `0` and an empty string disable the connector
- any other value enables the connector, for example `1` or `Y`

The string `N` is also treated as non-empty and enables the connector, so pass `0` to disable it.

The parameter must always be passed: without the `ACTIVE` key or with the `null` value, the method returns the `ERROR_ARGUMENT` error ||
|#

The method changes the connector state on the line:

- when enabling, it marks the connector as registered, connected, and active, and clears the error flag. In the response of the [imconnector.status](./imconnector-status.md) method, this results in `CONFIGURED: true` and `STATUS: true`
- when disabling, it deletes the connector status record for this line and triggers the [OnImConnectorStatusDelete](./events/on-im-connector-status-delete.md) event

The connector settings specified by the [imconnector.connector.data.set](./imconnector-connector-data-set.md) method are deleted along with the status record. After enabling the connector again, pass the settings once more.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CONNECTOR":"myconnector","LINE":107,"ACTIVE":"1","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imconnector.activate
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
        method: 'imconnector.activate',
        params: {
          CONNECTOR: 'myconnector',
          LINE: 107,
          ACTIVE: '1',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Connector activated:', result)
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
      async function activateConnector() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imconnector.activate',
            params: {
              CONNECTOR: 'myconnector',
              LINE: 107,
              ACTIVE: '1',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Connector activated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', activateConnector)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imconnector.activate(
            connector="myconnector",
            line=107,
            active=1,
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
        'imconnector.activate',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'ACTIVE' => '1',
        ]
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'imconnector.activate',
      {
        CONNECTOR: 'myconnector',
        LINE: 107,
        ACTIVE: '1',
      },
      function(result) {
        console.log(result.data());
      }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'imconnector.activate',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'ACTIVE' => '1',
        ]
    );
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imconnector.activate", b24.Params{
    	"CONNECTOR": "myconnector",
    	"LINE":      107,
    	"ACTIVE":    "1",
    })
    if err != nil {
    	return fmt.Errorf("imconnector.activate: %w", err)
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
[`boolean`](../../data-types.md) | `true` if the action was successful.

The method returns `true` even if the connector was already in the required state — when enabling it again or when disabling a connector that is not enabled on this line.

The value `false` is possible only when disabling, if the status record could not be deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'ACTIVE' is null or empty",
    "argument": "ACTIVE"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | Method called outside the application context OAuth ||
|| `400` | `ERROR_ARGUMENT` | Argument 'CONNECTOR' is null or empty | `CONNECTOR` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'LINE' is null or empty | `LINE` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'ACTIVE' is null or empty | The `ACTIVE` key is not provided or its value is `null` ||
|#

The body of the `ERROR_ARGUMENT` error contains an additional `argument` field with the parameter name — there is no need to parse the message text.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-connector-data-set.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)
- [{#T}](./events/on-im-connector-status-delete.md)
- [{#T}](../../../tutorials/openlines/example-connector.md)