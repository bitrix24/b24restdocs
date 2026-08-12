# Get Application-Linked Data app.option.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `app.option.get` retrieves data linked to the application. If no input is provided, it will return all properties recorded via [app.option.set](./app-option-set.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **option**
[`string`](../../data-types.md) | One of the keys saved by the [app.option.set](./app-option-set.md) method.

If the parameter is not provided, the method returns all saved application settings ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    Example №1

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "option": "data"
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/app.option.get
    ```

    Example №2

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/app.option.get
    ```

- cURL (OAuth)

    Example №1

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "option": "data",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/app.option.get
    ```
    
    Example №2
    
    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/app.option.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AppOptionResult = Record<string, string>

    // Example 1: get a specific option by key
    try {
      const response = await $b24.actions.v2.call.make<string | null>({
        method: 'app.option.get',
        params: {
          option: 'data',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Option value:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }

    // Example 2: get all options (no parameters)
    try {
      const response = await $b24.actions.v2.call.make<AppOptionResult>({
        method: 'app.option.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('All options:', Object.keys(result), result)
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
      async function getAppOptions() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // Example 1: get a specific option by key
          const response1 = await $b24.actions.v2.call.make({
            method: 'app.option.get',
            params: {
              option: 'data',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response1.isSuccess) {
            console.error(response1.getErrorMessages().join('; '))
            return
          }

          const result1 = response1.getData().result
          console.info('Option value:', result1)

          // Example 2: get all options (no parameters)
          const response2 = await $b24.actions.v2.call.make({
            method: 'app.option.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response2.isSuccess) {
            console.error(response2.getErrorMessages().join('; '))
            return
          }

          const result2 = response2.getData().result
          console.info('All options:', Object.keys(result2), result2)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAppOptions)
    </script>
    ```

- BX24.js

    Example №1

    ```js
    BX24.callMethod(
        'app.option.get',
        {
            "option":"data"
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```
    
    Example №2
    
    ```js
    BX24.callMethod(
        'app.option.get', {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    Example №1
    
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'app.option.get',
        [
            'option' => 'data'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

    Example №2
    
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'app.option.get',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.app.option.get(
            option="default_language",
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "app.option.get", b24.Params{
    	"option": "data",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("app.option.get: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

A call without the `option` parameter — the method returns all saved application settings:

```json
{
    "result": {
        "data": "value",
        "data2": "value2"
    },
    "time": {
        "start": 1722001311.94644,
        "finish": 1722001311.98622,
        "duration": 0.0397801399230957,
        "processing": 0.000041961669921875,
        "date_start": "2024-07-26T13:41:51+00:00",
        "date_finish": "2024-07-26T13:41:51+00:00",
        "operating": 0
    }
}
```

A call with the `option` parameter — the method returns the value of a single key:

```json
{
    "result": "value",
    "time": {
        "start": 1722001311.94644,
        "finish": 1722001311.98622,
        "duration": 0.0397801399230957,
        "processing": 0.000041961669921875,
        "date_start": "2024-07-26T13:41:51+00:00",
        "date_finish": "2024-07-26T13:41:51+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md)\|[`string`](../../data-types.md)\|[`null`](../../data-types.md) | Depends on the `option` parameter:

- the parameter is not provided — an object where the key is the setting name and the value is the saved value. If there are no settings, the object is empty
- the parameter is provided — the saved value of the key
- the parameter is provided, but there is no such key — `null` ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"AccessException",
    "error_description":"Application context required"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error message** | **Description** ||
|| `AccessException` | Application context required | The method is called outside the application context ||
|| `AccessException` | User authorization required | The user is not authorized ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./app-option-set.md)
- [{#T}](./user-option-set.md)
- [{#T}](./user-option-get.md)