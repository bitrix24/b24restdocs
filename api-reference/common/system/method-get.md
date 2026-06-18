# Get a list of available methods method.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `method.get` method returns two parameters `isExisting` and `isAvailable`, which determine the existence of the method on the account and its availability for invocation.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | The name of the method to check in lowercase, for example `user.get` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "name": "user.get"
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/method.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "name": "user.get",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/method.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MethodGetResult = {
      isExisting: boolean
      isAvailable: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<MethodGetResult>({
        method: 'method.get',
        params: {
          name: 'user.get',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('isExisting:', result.isExisting, 'isAvailable:', result.isAvailable)
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
      async function checkMethod() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'method.get',
            params: {
              name: 'user.get',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('isExisting:', result.isExisting, 'isAvailable:', result.isAvailable)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', checkMethod)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'method.get',
                [
                    'name' => 'user.get',
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
        echo 'Error calling method: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "method.get",
        {
            "name": "user.get"
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
        'method.get',
        [
            'name' => 'user.get'
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
    "result": {
        "isExisting": true,
        "isAvailable": true
    },
    "time": {
        "start": 1721983189.4629,
        "finish": 1721983189.50346,
        "duration": 0.0405528545379639,
        "processing": 0.000152111053466796,
        "date_start": "2024-07-26T08:39:49+00:00",
        "date_finish": "2024-07-26T08:39:49+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Two parameters are returned:

- `isExisting => true/false` — determines whether the method exists on this account
- `isAvailable => true/false` — determines the availability of the method for invocation with the current access permissions ([scope](./scope.md)) of the application ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}


## Continue Learning

- [{#T}](./scope.md)
- [{#T}](./app-info.md)
- [{#T}](./access-name.md)
- [{#T}](./feature-get.md)
- [{#T}](./server-time.md)
- [{#T}](./methods.md)