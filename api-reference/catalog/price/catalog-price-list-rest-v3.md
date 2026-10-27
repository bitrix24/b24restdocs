# Get Price List by Filter v 3.0 catalog.price.list

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

The method `catalog.price.list` returns a list of product prices with filtering, sorting, and pagination.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
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
|| **filter**
[`array`](../../data-types.md) | Filter in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`, for example `[["productId","=",7525]]`. You can filter by all fields from `select`.

The syntax and operators are described in the [REST 3.0 overview](../../rest-v3.md#filter) ||
|| **order**
[`object`](../../data-types.md) | Sorting in the `{"field": "direction"}` format. You can sort by all fields from `select`.

Possible direction values:

- `ASC` — ascending
- `DESC` — descending

Default: `{"id": "ASC"}` ||
|| **pagination**
[`object`](../../data-types.md) | Pagination object. [Object structure description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of prices per page. Values greater than `1000` are reduced to `1000`. Zero, negative, and non-numeric values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) × limit`.

If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../data-types.md) | Number of prices to skip from the beginning of the list.

Default: `0` ||
|#

{% note info "" %}

The response does not contain the total number of prices. If a page returns fewer prices than specified in `limit`, it is the last page.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.price.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["productId","=",7525]],"select":["id","catalogGroupId","price","currency"],"order":{"catalogGroupId":"asc"},"pagination":{"limit":10}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.price.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["productId","=",7525]],"select":["id","catalogGroupId","price","currency"],"order":{"catalogGroupId":"asc"},"pagination":{"limit":10},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.price.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PriceListResult = {
      items: Array<{
        id: number
        catalogGroupId: number
        price: number
        currency: string
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<PriceListResult>({
        method: 'catalog.price.list',
        params: {
          filter: [
            ['productId', '=', 7525],
          ],
          select: [
            'id',
            'catalogGroupId',
            'price',
            'currency',
          ],
          order: {
            catalogGroupId: 'asc',
          },
          pagination: {
            limit: 10,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Prices:', result.items)
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
      async function listPrices() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.price.list',
            params: {
              filter: [
                ['productId', '=', 7525],
              ],
              select: [
                'id',
                'catalogGroupId',
                'price',
                'currency',
              ],
              order: {
                catalogGroupId: 'asc',
              },
              pagination: {
                limit: 10,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Prices:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listPrices)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.price.list(
            filter=[
                ["productId", "=", 7525],
            ],
            select=[
                "id",
                "catalogGroupId",
                "price",
                "currency",
            ],
            order={
                "catalogGroupId": "asc",
            },
            pagination={
                "limit": 10,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.price.list',
                [
                    'filter' => [
                        ['productId', '=', 7525],
                    ],
                    'select' => [
                        'id',
                        'catalogGroupId',
                        'price',
                        'currency',
                    ],
                    'order' => [
                        'catalogGroupId' => 'asc',
                    ],
                    'pagination' => [
                        'limit' => 10,
                    ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.price.list',
        {
            filter: [
                ['productId', '=', 7525],
            ],
            select: [
                'id',
                'catalogGroupId',
                'price',
                'currency',
            ],
            order: {
                catalogGroupId: 'asc',
            },
            pagination: {
                limit: 10,
            },
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
        'catalog.price.list',
        [
            'filter' => [
                ['productId', '=', 7525],
            ],
            'select' => [
                'id',
                'catalogGroupId',
                'price',
                'currency',
            ],
            'order' => [
                'catalogGroupId' => 'asc',
            ],
            'pagination' => [
                'limit' => 10,
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
    res, err := client.Core().Call(ctx, "catalog.price.list", b24.Params{
    	"filter": []any{
    		[]any{"productId", "=", 7525},
    	},
    	"select": []string{"id", "catalogGroupId", "price", "currency"},
    	"order": b24.Params{
    		"catalogGroupId": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 10,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.price.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID             int     `json:"id"`
    		CatalogGroupID int     `json:"catalogGroupId"`
    		Price          float64 `json:"price"`
    		Currency       string  `json:"currency"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.CatalogGroupID, item.Price, item.Currency)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 891,
                "catalogGroupId": 1,
                "price": 1490,
                "currency": "EUR"
            },
            {
                "id": 893,
                "catalogGroupId": 3,
                "price": 1595,
                "currency": "EUR"
            }
        ]
    },
    "time": {
        "start": 1792668102,
        "finish": 1792668102.218043,
        "duration": 0.21804308891296387,
        "processing": 0,
        "date_start": "2026-10-22T14:21:42+02:00",
        "date_finish": "2026-10-22T14:21:42+02:00",
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
|| **result.items**
[`array`](../../data-types.md) | Array of prices. The set of fields of each price depends on `select`. [Field description](#items) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

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
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION",
        "message": "Invalid filter \"Unknown filter condition\"."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["productId","=",7525]]` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `PriceDto` | Specify only the fields from the list of the `select` parameter ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass an integer greater than zero in `limit` ||
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

- [{#T}](./catalog-price-get-rest-v3.md)
- [{#T}](./catalog-price-add-rest-v3.md)
- [{#T}](./catalog-price-update-rest-v3.md)
- [{#T}](./catalog-price-field-list.md)
