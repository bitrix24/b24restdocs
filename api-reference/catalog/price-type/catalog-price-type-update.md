# Update Price Type catalog.priceType.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method modifies the values of the price type fields.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_price_type.id`](../data-types.md#catalog_price_type) | Identifier of the price type ||
|| **fields***
[`object`](../../data-types.md) | Field values to update the price type ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Code of the price type ||
|| **base**
[`string`](../../data-types.md) | Indicates if the price type is base. Possible values:
- `Y` — yes
- `N` — no
||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|| **xmlId**
[`string`](../../data-types.md) | External code.

Can be used to synchronize the current price type with a similar position in an external system
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2,"fields":{"name":"Base wholesale price","base":"Y","sort":1,"xmlId":"basewholesale"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceType.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2,"fields":{"name":"Base wholesale price","base":"Y","sort":1,"xmlId":"basewholesale"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceType.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PriceTypeUpdateResult = {
      priceType: {
        base: string
        createdBy: number
        dateCreate: ISODate | null
        id: number
        modifiedBy: number
        name: string
        sort: number
        timestampX: ISODate | null
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<PriceTypeUpdateResult>({
        method: 'catalog.priceType.update',
        params: {
          id: 2,
          fields: {
            name: 'Base wholesale price',
            base: 'Y',
            sort: 1,
            xmlId: 'basewholesale',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.priceType.id, result.priceType.name)
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
      async function updatePriceType() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.priceType.update',
            params: {
              id: 2,
              fields: {
                name: 'Base wholesale price',
                base: 'Y',
                sort: 1,
                xmlId: 'basewholesale',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.priceType.id, result.priceType.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updatePriceType)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.price_type.update(
            bitrix_id=2,
            fields={
                "name": "Base wholesale price",
                "base": "Y",
                "sort": 1,
                "xmlId": "basewholesale",
            },
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
                'catalog.priceType.update',
                [
                    'id' => 2,
                    'fields' => [
                        'name'  => "Base wholesale price",
                        'base'  => "Y",
                        'sort'  => 1,
                        'xmlId' => "basewholesale",
                    ],
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
        echo 'Error updating price type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.priceType.update', 
        {
            id: 2,
            fields: {
                name: "Base wholesale price",
                base: "Y",
                sort: 1,
                xmlId: "basewholesale"
            }
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
        'catalog.priceType.update',
        [
            'id' => 2,
            'fields' => [
                'name' => 'Base wholesale price',
                'base' => 'Y',
                'sort' => 1,
                'xmlId' => 'basewholesale'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.priceType.update", b24.Params{
    	"id": 2,
    	"fields": b24.Params{
    		"name":  "Base wholesale price",
    		"base":  "Y",
    		"sort":  1,
    		"xmlId": "basewholesale",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.priceType.update: %w", err)
    }

    // The method wraps the response in an object with the "priceType" key.
    raw, ok := b24.Unwrap(res.Result, "priceType")
    if !ok {
    	return fmt.Errorf("no priceType key in the response")
    }

    var item struct {
    	Base       string `json:"base"`
    	CreatedBy  int    `json:"createdBy"`
    	DateCreate string `json:"dateCreate"`
    	ID         b24.ID `json:"id"`
    	ModifiedBy int    `json:"modifiedBy"`
    	Name       string `json:"name"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Base, item.CreatedBy)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "priceType": {
            "base": "Y",
            "createdBy": 1,
            "dateCreate": "2024-10-02T17:49:44+02:00",
            "id": 2,
            "modifiedBy": 1,
            "name": "Base wholesale price",
            "sort": 1,
            "timestampX": "2024-10-03T12:29:35+02:00",
            "xmlId": "basewholesale"
        }
    },
    "time": {
        "start": 1712327086.69665,
        "finish": 1712327086.95303,
        "duration": 0.256376028060913,
        "processing": 0.0112268924713135,
        "date_start": "2024-10-03T12:29:35+02:00",
        "date_finish": "2024-10-03T12:29:35+02:00",
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
|| **priceType**
[`catalog_price_type`](../data-types.md#catalog_price_type) | Object with information about the updated price type ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description":"Required fields: name"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to edit
||
|| `201000000000` | Price type with such identifier does not exist
||
|| `100` | Parameter `id` not specified
||
|| `100` | Parameter `fields` not specified or empty
||
|| `0` | Required fields of the `fields` structure not provided
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-add.md)
- [{#T}](./catalog-price-type-get.md)
- [{#T}](./catalog-price-type-list.md)
- [{#T}](./catalog-price-type-delete.md)
- [{#T}](./catalog-price-type-get-fields.md)