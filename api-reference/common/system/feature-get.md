# Get information about feature availability on the account feature.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `feature.get` returns information about the availability of features on a specific account.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../data-types.md) | Available keys:
- `rest_offline_extended` — availability of offline events
- `rest_auth_connector` — availability of the `auth_connector` key in events ||
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
        "CODE": "rest_offline_extended"
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/feature.get
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "CODE": "rest_offline_extended",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/feature.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FeatureGetResult = {
      value: string // 'Y' | 'N'
    }

    try {
      const response = await $b24.actions.v2.call.make<FeatureGetResult>({
        method: 'feature.get',
        params: {
          CODE: 'rest_offline_extended',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Feature availability:', result.value)
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
      async function getFeature() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'feature.get',
            params: {
              CODE: 'rest_offline_extended',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Feature availability:', result.value)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFeature)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'feature.get',
                [
                    'CODE' => 'rest_offline_extended'
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
        echo 'Error getting feature: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "feature.get",
        {
            "CODE": "rest_offline_extended"
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
        'feature.get',
        [
            'CODE' => 'rest_offline_extended'
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
        "value": "Y"
    },
    "time": {
        "start": 1722434594.84942,
        "finish": 1722434594.90542,
        "duration": 0.0560011863708496,
        "processing": 0.000065088272094726,
        "date_start": "2024-07-31T14:03:14+00:00",
        "date_finish": "2024-07-31T14:03:14+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The object contains information about the method's availability:
- `value` — (Y/N) presence of the feature on the account
- `lang_selfhosted` — *lang* is replaced with en, de, ua, kz, etc. (used for on-premise *Bitrix24*) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"CODE_EMPTY",
    "error_description":"CODE can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %} 

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `CODE_EMPTY` | CODE can't be empty | The CODE parameter was not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./method-get.md)
- [{#T}](./scope.md)
- [{#T}](./app-info.md)
- [{#T}](./access-name.md)
- [{#T}](./server-time.md)
- [{#T}](./methods.md)