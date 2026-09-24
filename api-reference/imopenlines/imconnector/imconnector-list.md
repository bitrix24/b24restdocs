# Get the List of Connectors imconnector.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify Open Channels connectors

The method `imconnector.list` returns a list of connectors that are available in Bitrix24 and can be connected to an open line.

{% note info "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md).

{% endnote %}

The list includes:

- built-in connectors that are available in the region and enabled in the Bitrix24 settings: Live Chat, Telegram, Bitrix24 Network, and others
- custom connectors registered by applications via [imconnector.register](./imconnector-register.md)

Bitrix24 settings do not affect custom connectors: they appear in the list right after registration.

The method does not show which lines the connectors are attached to or what state they are in. To check the connector state on a specific line, use the [imconnector.status](./imconnector-status.md) method.

## Method Parameters

No parameters.

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imconnector.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ConnectorListResult = Record<string, string>

    try {
      const response = await $b24.actions.v2.call.make<ConnectorListResult>({
        method: 'imconnector.list',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Connectors:', result)
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
      async function listConnectors() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imconnector.list',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Connectors:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listConnectors)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imconnector.list().response
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
        'imconnector.list',
        []
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'imconnector.list',
      {},
      function(result) {
        console.log(result.data());
      }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'imconnector.list',
        []
    );
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imconnector.list", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imconnector.list: %w", err)
    }

    var item struct {
    	Livechat    string `json:"livechat"`
    	Telegrambot string `json:"telegrambot"`
    	Network     string `json:"network"`
    	Myconnector string `json:"myconnector"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Livechat, item.Telegrambot)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "livechat": "Live Chat",
        "telegrambot": "Telegram",
        "network": "Bitrix24 Network",
        "myconnector": "My Connector"
    },
    "time": {
        "start": 1738065600.11,
        "finish": 1738065600.17,
        "duration": 0.06,
        "processing": 0.03,
        "date_start": "2025-01-28T12:00:00+00:00",
        "date_finish": "2025-01-28T12:00:00+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object of the form `connector_id: connector_name`, where:

- the key is the connector code. For a custom connector, this is the value of the `ID` parameter of the [imconnector.register](./imconnector-register.md) method converted to lowercase
- the value is the connector name in the Bitrix24 interface language: for a custom connector, this is the `NAME` parameter of the [imconnector.register](./imconnector-register.md) method

The key is passed in the `CONNECTOR` parameter of the other methods in this section.

If there are no available connectors, `result` contains an empty array `[]` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

Called outside the application context:

```json
{
    "error": "ERROR_CORE",
    "error_description": "Current authorization type is denied for this method"
}
```

No permission to modify connectors:

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "You don't have access to this action"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CORE` | Current authorization type is denied for this method | The method was called outside of the application OAuth context. Unlike the other methods in this section, this method responds with the `ERROR_CORE` code and the `400` status ||
|| `400` | `ACCESS_DENIED` | The ImOpenLines module is not installed. | The `imopenlines` module is not installed in Bitrix24. This check runs before the application context check ||
|| `400` | `ACCESS_DENIED` | You don't have access to this action | The user does not have permission to modify connectors ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-connector-data-set.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)
- [{#T}](../../../tutorials/openlines/example-connector.md)