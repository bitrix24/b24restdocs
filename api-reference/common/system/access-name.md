# Get Access Permission Names access.name

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `access.name` retrieves the names of access permissions.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ACCESS***
[`array`](../../data-types.md) | List of permission identifiers for which names need to be retrieved ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "ACCESS": ["G2", "AU"]
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/access.name
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "ACCESS": ["G2", "AU"],
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/access.name
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type AccessNameItem = {
      provider: string
      name: string
      provider_id: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AccessNameResult = Record<string, AccessNameItem>

    try {
      const response = await $b24.actions.v2.call.make<AccessNameResult>({
        method: 'access.name',
        params: {
          ACCESS: ['G2', 'AU'],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result), result)
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
      async function getAccessNames() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'access.name',
            params: {
              ACCESS: ['G2', 'AU'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result), result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAccessNames)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'access.name',
                [
                    'ACCESS' => ['G2', 'AU']
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
        echo 'Error calling access.name: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "access.name",
        {
            "ACCESS": ["G2", "AU"]
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
        'access.name',
        [
            'ACCESS' => ['G2','AU']
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
    "result": {
        "G2": {
            "provider": "",
            "name": "All visitors",
            "provider_id": "other"
        },
        "AU": {
            "provider": "",
            "name": "All authorized users",
            "provider_id": "other"
        }
    },
    "time": {
        "start": 1722002504.2838,
        "finish": 1722002504.32483,
        "duration": 0.0410301685333252,
        "processing": 0.00145506858825684,
        "date_start": "2024-07-26T14:01:44+00:00",
        "date_finish": "2024-07-26T14:01:44+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Objects describing permissions ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./method-get.md)
- [{#T}](./scope.md)
- [{#T}](./app-info.md)
- [{#T}](./feature-get.md)
- [{#T}](./server-time.md)
- [{#T}](./methods.md)