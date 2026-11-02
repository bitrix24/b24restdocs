# Get Values of All Fields in the Trade Catalog catalog.catalog.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `catalog.catalog.get` returns the field values of a trade catalog by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Trade catalog identifier. It matches the identifier of the catalog information block.

You can get the identifier using the [catalog.catalog.list](./catalog-catalog-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":25}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.catalog.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":25,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.catalog.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CatalogGetResult = {
      catalog: {
        iblockId: number
        iblockTypeId: string
        id: number
        lid: string
        name: string
        productIblockId: number | null
        skuPropertyId: number | null
        subscription: 'Y' | 'N'
        vatId: number | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<CatalogGetResult>({
        method: 'catalog.catalog.get',
        params: {
          id: 25,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.catalog)
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
      async function getCatalog() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.catalog.get',
            params: {
              id: 25,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.catalog)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCatalog)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.catalog.get(
            bitrix_id=25,
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
                'catalog.catalog.get',
                [
                    'id' => 25
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting catalog: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.catalog.get', {
            'id': 25
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.catalog.get',
        [
            'id' => 25
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.catalog.get", b24.Params{
    	"id": 25,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.catalog.get: %w", err)
    }

    // The method wraps the response in an object with the "catalog" key.
    raw, ok := b24.Unwrap(res.Result, "catalog")
    if !ok {
    	return fmt.Errorf("no catalog key in the response")
    }

    var item struct {
    	IblockID     b24.ID `json:"iblockId"`
    	IblockTypeID string `json:"iblockTypeId"`
    	ID           b24.ID `json:"id"`
    	Lid          string `json:"lid"`
    	Name         string `json:"name"`
    	Subscription string `json:"subscription"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.IblockID, item.IblockTypeID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "catalog": {
            "iblockId": 25,
            "iblockTypeId": "CRM_PRODUCT_CATALOG",
            "id": 25,
            "lid": "s1",
            "name": "CRM product catalog",
            "productIblockId": null,
            "skuPropertyId": null,
            "subscription": "N",
            "vatId": 1
        }
    },
    "time": {
        "start": 1793612963,
        "finish": 1793612963.560481,
        "duration": 0.560481071472168,
        "processing": 0,
        "date_start": "2026-11-02T12:49:23+03:00",
        "date_finish": "2026-11-02T12:49:23+03:00",
        "operating_reset_at": 1793613540,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.catalog**
[`catalog_catalog`](../data-types.md#catalog_catalog) | Object with information about the trade catalog ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "catalog does not exist."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user does not have permission to view Bitrix24 settings ||
|| `400` | `200040300030` | Access Denied | The user does not have permission to read the trade catalog ||
|| `400` | `100` | Could not find value for parameter {id} | The required parameter `id` is not passed ||
|| `400` | Empty value | catalog does not exist. | The trade catalog with the specified `id` was not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-catalog-list.md)
- [{#T}](./catalog-catalog-is-offers.md)
- [{#T}](./catalog-catalog-get-fields.md)