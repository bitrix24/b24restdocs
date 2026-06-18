# Get a List of Available Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `methods` method retrieves a list of available methods.

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [method.get](./method-get.md).

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **full**
[`boolean`](../../data-types.md) | If the parameter is set to `true`, the method will return a list of all methods ||
|| **scope**
[`string`](../../data-types.md) | Displays methods included in the specified permission. If the parameter is provided without a value (`methods?scope=&auth=xxxxx`), all common methods will be displayed. ||
|#

> If the method is called without parameters, it will return a list of all methods available to the current application.

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "scope": "user"
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/methods
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "scope": "user",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/methods
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<string[]>({
        method: 'methods',
        params: {
          scope: 'user',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.length, result)
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
      async function fetchMethods() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'methods',
            params: {
              scope: 'user',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchMethods)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'methods',
                [
                    'scope' => 'user',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling method: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "methods",
        {
            "scope": "user"
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'methods',
        [
            'scope' => 'user'
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
    "result": [
        "user.fields",
        "user.current",
        "user.get",
        "user.search",
        "user.add",
        "user.update",
        "user.online",
        "user.counters",
        "user.history.list",
        "user.history.fields.list"
    ],
    "time": {
        "start": 1721986432.32646,
        "finish": 1721986432.3598,
        "duration": 0.0333478450775147,
        "processing": 0.000032901763916015,
        "date_start": "2024-07-26T09:33:52+00:00",
        "date_finish": "2024-07-26T09:33:52+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | An array containing the list of permissions ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./method-get.md)
- [{#T}](./scope.md)
- [{#T}](./app-info.md)
- [{#T}](./access-name.md)
- [{#T}](./feature-get.md)
- [{#T}](./server-time.md)