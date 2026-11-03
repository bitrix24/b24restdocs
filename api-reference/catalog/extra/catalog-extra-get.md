# Get Margin Field Values by ID catalog.extra.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "Edit selling price", or "Manage mark-ups" access permission

`catalog.extra.get` returns information about a markup by its ID.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **id*** 
[`catalog_extra.id`](../data-types.md#catalog_extra) | Markup ID.

You can retrieve the ID using the [catalog.extra.list](./catalog-extra-list.md) method ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.extra.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.extra.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ExtraGetResult = {
      extra: {
        id: number
        name: string
        percentage: number
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ExtraGetResult>({
        method: 'catalog.extra.get',
        params: {
          id: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.extra.id, result.extra.name, result.extra.percentage)
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
      async function getExtra() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.extra.get',
            params: {
              id: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.extra.id, result.extra.name, result.extra.percentage)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getExtra)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.extra.get(
            bitrix_id=1,
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
        $response = $b24Service
            ->core
            ->call(
                'catalog.extra.get',
                [
                    'id' => 1
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.extra.get',
        {
            id: 1
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
        'catalog.extra.get',
        [
            'id' => 1
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.extra.get", b24.Params{
    	"id": 1,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.extra.get: %w", err)
    }

    // The method wraps the response in an object with the "extra" key.
    raw, ok := b24.Unwrap(res.Result, "extra")
    if !ok {
    	return fmt.Errorf("no extra key in the response")
    }

    var item struct {
    	ID         b24.ID  `json:"id"`
    	Name       string  `json:"name"`
    	Percentage float64 `json:"percentage"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "extra": {
            "id": 1,
            "name": "Markup1",
            "percentage": 10
        }
    },
    "time": {
        "start": 1793688334,
        "finish": 1793688334.074482,
        "duration": 0.07448196411132812,
        "processing": 0,
        "date_start": "2026-11-03T09:45:34+03:00",
        "date_finish": "2026-11-03T09:45:34+03:00",
        "operating_reset_at": 1793688840,
        "operating": 0.1182100772857666
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.extra**
[`catalog_extra`](../data-types.md#catalog_extra) | Object containing information about the margin with the specified ID ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "202000000000",
    "error_description": "extra does not exist."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user does not have the "View product catalog", "Edit selling price", or "Manage mark-ups" access permission ||
|| `400` | `100` | Could not find value for parameter {id} | The required parameter `id` is not passed ||
|| `400` | `202000000000` | extra does not exist. | There is no markup with the specified `id` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-extra-list.md)
- [{#T}](./catalog-extra-get-fields.md)