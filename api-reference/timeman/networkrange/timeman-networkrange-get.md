# Get Network Address Ranges timeman.networkrange.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `timeman.networkrange.get` retrieves the ranges of network addresses for the office network.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.networkrange.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.networkrange.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each NetworkRangeItem returned in result[]
    type NetworkRangeItem = {
      ip_range: string
      name: string
    }

    try {
      const response = await $b24.actions.v2.call.make<NetworkRangeItem[]>({
        method: 'timeman.networkrange.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Network ranges:', result.map(r => `${r.name}: ${r.ip_range}`))
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
      async function getNetworkRanges() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'timeman.networkrange.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Network ranges:', result.map(r => `${r.name}: ${r.ip_range}`))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getNetworkRanges)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.networkrange.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error()->ex;
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting network range: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'timeman.networkrange.get',
        {},
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.networkrange.get',
        []
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
    "result": [
        {
            "ip_range": "10.0.0.0-10.255.255.255",
            "name": "10.x.x.x"
        },
        {
            "ip_range": "172.16.0.0-172.31.255.255",
            "name": "172.x.x.x"
        },
        {
            "ip_range": "192.168.0.0-192.168.255.255",
            "name": "192.168.x.x"
        }
    ],
    "time": {
        "start": 1742999109.792954,
        "finish": 1742999109.827862,
        "duration": 0.034908056259155273,
        "processing": 0.0008471012115478516,
        "date_start": "2025-03-26T17:25:09+02:00",
        "date_finish": "2025-03-26T17:25:09+02:00",
        "operating_reset_at": 1742999709,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | The root element of the response.

Contains a list of objects. Each object contains a description of the [network address range](#ip_range) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Range Object {#ip_range}

#|
|| **Name**
`type` | **Description** ||
|| **ip_range**
 [`string`](../../data-types.md) | The range of network addresses ||
|| **name**
 [`string`](../../data-types.md) | The name of the range ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You don't have access to use this method"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_ERROR` | You don't have access to use this method | The method is only available to the administrator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-networkrange-set.md)
- [{#T}](./timeman-networkrange-check.md)
