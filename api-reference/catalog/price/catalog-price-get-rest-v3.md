# Get Product Price by ID v 3.0 catalog.price.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "Edit selling price" access permission

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.price.get` returns the fields of a product price by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Price identifier.

The identifier can be retrieved using the [catalog.price.list](./catalog-price-list-rest-v3.md) method ||
|| **select**
[`array`](../../data-types.md) | Array of price fields to return in the response.

Available fields:

- `id` — price identifier
- `productId` — identifier of the product or variation
- `catalogGroupId` — price type identifier
- `price` — price
- `currency` — currency code
- `extraId` — markup identifier
- `quantityFrom` — lower bound of the quantity range
- `quantityTo` — upper bound of the quantity range
- `priceScale` — price in the base currency
- `timestampX` — date and time of the last change

The `id` field is always returned, even if it is not in `select`.

Default: all fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.price.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":891,"select":["id","productId","catalogGroupId","price","currency","priceScale"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.price.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":891,"select":["id","productId","catalogGroupId","price","currency","priceScale"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.price.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PriceGetResult = {
      item: {
        id: number
        productId: number
        catalogGroupId: number
        price: number
        currency: string
        priceScale: number
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<PriceGetResult>({
        method: 'catalog.price.get',
        params: {
          id: 891,
          select: [
            'id',
            'productId',
            'catalogGroupId',
            'price',
            'currency',
            'priceScale',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Price:', result.item.price, result.item.currency)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getPrice() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.price.get',
            params: {
              id: 891,
              select: [
                'id',
                'productId',
                'catalogGroupId',
                'price',
                'currency',
                'priceScale',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Price:', result.item.price, result.item.currency)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPrice)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.price.get(
            bitrix_id=891,
            select=[
                "id",
                "productId",
                "catalogGroupId",
                "price",
                "currency",
                "priceScale",
            ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.price.get',
                [
                    'id' => 891,
                    'select' => [
                        'id',
                        'productId',
                        'catalogGroupId',
                        'price',
                        'currency',
                        'priceScale',
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
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.price.get',
        {
            id: 891,
            select: [
                'id',
                'productId',
                'catalogGroupId',
                'price',
                'currency',
                'priceScale',
            ],
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.price.get',
        [
            'id' => 891,
            'select' => [
                'id',
                'productId',
                'catalogGroupId',
                'price',
                'currency',
                'priceScale',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.price.get", b24.Params{
    	"id":     891,
    	"select": []string{"id", "productId", "catalogGroupId", "price", "currency", "priceScale"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.price.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID             int     `json:"id"`
    	ProductID      int     `json:"productId"`
    	CatalogGroupID int     `json:"catalogGroupId"`
    	Price          float64 `json:"price"`
    	Currency       string  `json:"currency"`
    	PriceScale     float64 `json:"priceScale"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Price, item.Currency)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 891,
            "productId": 7525,
            "catalogGroupId": 1,
            "price": 1490,
            "currency": "EUR",
            "priceScale": 1490
        }
    },
    "time": {
        "start": 1792668101,
        "finish": 1792668101.356511,
        "duration": 0.35651111602783203,
        "processing": 0,
        "date_start": "2026-10-22T14:21:41+02:00",
        "date_finish": "2026-10-22T14:21:41+02:00",
        "operating_reset_at": 1792668660,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | Price fields. The set depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Price identifier ||
|| **productId**
[`integer`](../../data-types.md) | Identifier of the product or variation ||
|| **catalogGroupId**
[`integer`](../../data-types.md) | Price type identifier ||
|| **price**
[`double`](../../data-types.md) | Price ||
|| **currency**
[`string`](../../data-types.md) | Currency code ||
|| **extraId**
[`integer`](../../data-types.md) | Markup identifier. `0` if no markup is set ||
|| **quantityFrom**
[`integer`](../../data-types.md) | Lower bound of the quantity range. `0` if the bound is not set ||
|| **quantityTo**
[`integer`](../../data-types.md) | Upper bound of the quantity range. `0` if the bound is not set ||
|| **priceScale**
[`double`](../../data-types.md) | Price in the base currency. Calculated automatically ||
|| **timestampX**
[`datetime`](../../data-types.md) | Date and time of the last price change ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 99999999 was not found."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass a positive number in `id` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no price with this identifier. Check the identifier using the [catalog.price.list](./catalog-price-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `PriceDto` | Specify in `select` only the fields from the list of the `select` parameter ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" or "Edit selling price" access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-list-rest-v3.md)
- [{#T}](./catalog-price-update-rest-v3.md)
- [{#T}](./catalog-price-delete-rest-v3.md)
- [{#T}](./catalog-price-field-list.md)
