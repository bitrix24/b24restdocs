# Get List of Products by Filter v 3.0 catalog.product.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.list` method returns a list of trade catalog products by filter.

{% note info "" %}

The method returns products of any type: simple products, products with variations, variations, and services. To retrieve products of one type, add a condition on the `type` field to the filter.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../data-types.md) | List filter in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`. The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter).

The `["iblockId", "=", <identifier>]` condition is required. The information block identifier can be retrieved using the [catalog.catalog.list](../catalog/catalog-catalog-list-rest-v3.md) method.

You can filter by the `id`, `iblockId`, `name`, `code`, `xmlId`, `active`, `sort`, `createdBy`, `modifiedBy`, `dateCreate`, `timestampX`, `dateActiveFrom`, `dateActiveTo`, `iblockSectionId`, `type`, `available`, `bundle`, `quantity`, `quantityReserved`, `quantityTrace`, `canBuyZero`, `subscribe`, `vatId`, `vatIncluded`, `purchasingPrice`, `purchasingCurrency`, `barcodeMulti`, `weight`, `length`, `width`, `height`, `measure` fields. Filtering by property values is not supported.

Groups with `or` logic and negation are not supported. An `in` condition with an empty list returns an empty list ||
|| **select**
[`array`](../../data-types.md) | An array of product fields to return in the response.

Available fields:

- `id` — product identifier
- `iblockId` — information block identifier
- `name` — name
- `code` — symbolic code
- `xmlId` — external code
- `active` — activity
- `sort` — sort order
- `createdBy` — created by
- `modifiedBy` — modified by
- `dateCreate` — creation date
- `timestampX` — modification date
- `dateActiveFrom` — activity start
- `dateActiveTo` — activity end
- `previewText` — preview text
- `previewTextType` — preview text type
- `previewPicture` — preview picture
- `detailText` — detail text
- `detailTextType` — detail text type
- `detailPicture` — detail picture
- `iblockSectionId` — main section
- `iblockSection` — all sections
- `type` — product type
- `available` — availability for purchase
- `bundle` — bundle flag
- `quantity` — quantity
- `quantityReserved` — reserved quantity
- `quantityTrace` — quantity tracking
- `canBuyZero` — purchase when out of stock
- `subscribe` — subscription to the product
- `vatId` — VAT rate
- `vatIncluded` — VAT included in the price
- `purchasingPrice` — purchase price
- `purchasingCurrency` — purchase price currency
- `barcodeMulti` — barcode for each item
- `weight` — weight
- `length` — length
- `width` — width
- `height` — height
- `measure` — unit of measurement
- `recurSchemeLength` — payment period length
- `recurSchemeType` — payment period unit
- `trialPriceId` — product for trial payment
- `withoutOrder` — renewal without an order
- `quantityTraceRaw` — saved tracking mode
- `paymentType` — payment type
- `subscribeRaw` — saved subscription permission
- `canBuyZeroRaw` — saved permission to purchase when out of stock
- `properties` — values of all product properties

The `id` field is returned only if it is specified in `select`. Values of individual properties cannot be selected: `properties` returns all product properties. The `["*"]` value is not supported.

Default: all fields ||
|| **order**
[`object`](../../data-types.md) | Sorting in the `{"field": "direction"}` format, for example `{"id": "desc"}`.

Possible direction values:

- `asc` — ascending
- `desc` — descending

You can sort by the `id`, `iblockId`, `name`, `code`, `xmlId`, `active`, `sort`, `dateCreate`, `timestampX`, `dateActiveFrom`, `dateActiveTo`, `type`, `quantity` fields.

Default: `{"id": "asc"}` ||
|| **pagination**
[`object`](../../data-types.md) | Pagination. [Parameter description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of products per page. Values greater than `50` are reduced to `50`, and the `0` value causes an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1` ||
|| **offset**
[`integer`](../../data-types.md) | Number of products to skip. The value must be a multiple of `limit`, otherwise the method returns an error ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["iblockId","=",25],["iblockSectionId","=",41]],"select":["id","name","active","type"],"order":{"id":"desc"},"pagination":{"limit":2}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["iblockId","=",25],["iblockSectionId","=",41]],"select":["id","name","active","type"],"order":{"id":"desc"},"pagination":{"limit":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductListResult = {
      items: Array<{
        id: number
        name: string
        active: 'Y' | 'N'
        type: number
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductListResult>({
        method: 'catalog.product.list',
        params: {
          filter: [
            ['iblockId', '=', 25],
            ['iblockSectionId', '=', 41],
          ],
          select: ['id', 'name', 'active', 'type'],
          order: {
            id: 'desc',
          },
          pagination: {
            limit: 2,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Products:', result.items.length)
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
      async function productList() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.list',
            params: {
              filter: [
                ['iblockId', '=', 25],
                ['iblockSectionId', '=', 41],
              ],
              select: ['id', 'name', 'active', 'type'],
              order: {
                id: 'desc',
              },
              pagination: {
                limit: 2,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Products:', result.items.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', productList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.list(
            filter=[
                ["iblockId", "=", 25],
                ["iblockSectionId", "=", 41],
            ],
            select=["id", "name", "active", "type"],
            order={
                "id": "desc",
            },
            pagination={
                "limit": 2,
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
                'catalog.product.list',
                [
                    'filter' => [
                        ['iblockId', '=', 25],
                        ['iblockSectionId', '=', 41],
                    ],
                    'select' => ['id', 'name', 'active', 'type'],
                    'order' => [
                        'id' => 'desc',
                    ],
                    'pagination' => [
                        'limit' => 2,
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
        'catalog.product.list',
        {
            filter: [
                ['iblockId', '=', 25],
                ['iblockSectionId', '=', 41],
            ],
            select: ['id', 'name', 'active', 'type'],
            order: {
                id: 'desc',
            },
            pagination: {
                limit: 2,
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
        'catalog.product.list',
        [
            'filter' => [
                ['iblockId', '=', 25],
                ['iblockSectionId', '=', 41],
            ],
            'select' => ['id', 'name', 'active', 'type'],
            'order' => [
                'id' => 'desc',
            ],
            'pagination' => [
                'limit' => 2,
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
    res, err := client.Core().Call(ctx, "catalog.product.list", b24.Params{
    	"filter":    []any{
    		[]any{"iblockId", "=", 25},
    		[]any{"iblockSectionId", "=", 41},
    	},
    	"select":    []string{"id", "name", "active", "type"},
    	"order":     b24.Params{
    		"id":"desc",
    	},
    	"pagination":b24.Params{
    		"limit":2,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID     int    `json:"id"`
    		Name   string `json:"name"`
    		Active string `json:"active"`
    		Type   int    `json:"type"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Name, item.Type)
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
                "id": 7549,
                "name": "Basic T-shirt, white",
                "active": "Y",
                "type": 1
            },
            {
                "id": 7527,
                "name": "Cotton T-shirt",
                "active": "Y",
                "type": 1
            }
        ]
    },
    "time": {
        "start": 1792660648,
        "finish": 1792660648.515471,
        "duration": 0.5154709815979004,
        "processing": 0,
        "date_start": "2026-10-22T12:17:28+02:00",
        "date_finish": "2026-10-22T12:17:28+02:00",
        "operating_reset_at": 1792661220,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | An array of products. The set of fields of each product depends on `select`. [Field description](#items) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Product identifier ||
|| **iblockId**
[`integer`](../../data-types.md) | Identifier of the trade catalog information block ||
|| **name**
[`string`](../../data-types.md) | Product name ||
|| **code**
[`string`](../../data-types.md) or `null` | Symbolic code ||
|| **xmlId**
[`string`](../../data-types.md) | External code. If not set on creation, it matches `id` ||
|| **active**
[`string`](../../data-types.md) | Product activity: `Y` — active, `N` — inactive ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **createdBy**
[`integer`](../../data-types.md) | Identifier of the user who created the product ||
|| **modifiedBy**
[`integer`](../../data-types.md) | Identifier of the user who last modified the product ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Creation date and time in ISO 8601 format ||
|| **timestampX**
[`datetime`](../../data-types.md) | Date and time of the last modification in ISO 8601 format ||
|| **dateActiveFrom**
[`datetime`](../../data-types.md) or `null` | Activity start date ||
|| **dateActiveTo**
[`datetime`](../../data-types.md) or `null` | Activity end date ||
|| **previewText**
[`string`](../../data-types.md) or `null` | Preview text ||
|| **previewTextType**
[`string`](../../data-types.md) | Preview text type: `text` — text, `html` — HTML ||
|| **previewPicture**
[`string`](../../data-types.md) or `null` | Identifier of the preview picture file. The file can be retrieved using the [catalog.product.download](./catalog-product-download-rest-v3.md) method ||
|| **detailText**
[`string`](../../data-types.md) or `null` | Detail text ||
|| **detailTextType**
[`string`](../../data-types.md) | Detail text type: `text` — text, `html` — HTML ||
|| **detailPicture**
[`string`](../../data-types.md) or `null` | Identifier of the detail picture file. The file can be retrieved using the [catalog.product.download](./catalog-product-download-rest-v3.md) method ||
|| **iblockSectionId**
[`integer`](../../data-types.md) or `null` | Identifier of the product's main section ||
|| **iblockSection**
[`array`](../../data-types.md) | An array of identifiers of all sections the product is linked to ||
|| **type**
[`integer`](../../data-types.md) | Product type. The list of types is returned by the [catalog.product.type.list](./catalog-product-type-list.md) method. For a product created with `catalog.product.add`, the value is `1` ||
|| **available**
[`string`](../../data-types.md) | Availability for purchase: `Y` — available, `N` — unavailable ||
|| **bundle**
[`string`](../../data-types.md) | Bundle flag: `Y` — yes, `N` — no ||
|| **quantity**
[`double`](../../data-types.md) | Available quantity ||
|| **quantityReserved**
[`double`](../../data-types.md) | Reserved quantity ||
|| **quantityTrace**
[`string`](../../data-types.md) | Effective quantity tracking mode: `Y` — enabled, `N` — disabled ||
|| **canBuyZero**
[`string`](../../data-types.md) | Effective permission to purchase an out-of-stock product: `Y` — yes, `N` — no ||
|| **subscribe**
[`string`](../../data-types.md) | Effective permission to subscribe to the product: `Y` — yes, `N` — no ||
|| **vatId**
[`integer`](../../data-types.md) or `null` | VAT rate identifier ||
|| **vatIncluded**
[`string`](../../data-types.md) | VAT is included in the price: `Y` — yes, `N` — no ||
|| **purchasingPrice**
[`double`](../../data-types.md) or `null` | Purchase price ||
|| **purchasingCurrency**
[`string`](../../data-types.md) or `null` | Purchase price currency ||
|| **barcodeMulti**
[`string`](../../data-types.md) | Unique barcode for each item: `Y` — yes, `N` — no ||
|| **weight**
[`double`](../../data-types.md) | Weight ||
|| **length**
[`double`](../../data-types.md) or `null` | Length ||
|| **width**
[`double`](../../data-types.md) or `null` | Width ||
|| **height**
[`double`](../../data-types.md) or `null` | Height ||
|| **measure**
[`integer`](../../data-types.md) or `null` | Unit of measurement identifier ||
|| **recurSchemeLength**
[`integer`](../../data-types.md) or `null` | Payment period length. Used only in the on-premise version for content sales ||
|| **recurSchemeType**
[`string`](../../data-types.md) | Time unit of the payment period. Used only in the on-premise version for content sales ||
|| **trialPriceId**
[`integer`](../../data-types.md) or `null` | Identifier of the product for trial payment. Used only in the on-premise version for content sales ||
|| **withoutOrder**
[`string`](../../data-types.md) | Renewal without placing an order: `Y` — yes, `N` — no. Used only in the on-premise version for content sales ||
|| **quantityTraceRaw**
[`string`](../../data-types.md) | Saved value of the quantity tracking mode: `Y`, `N`, or `D` — the value from the module settings ||
|| **paymentType**
[`string`](../../data-types.md) | Payment type. Used only in the on-premise version for content sales ||
|| **subscribeRaw**
[`string`](../../data-types.md) | Saved value of the subscription permission: `Y`, `N`, or `D` — the value from the module settings ||
|| **canBuyZeroRaw**
[`string`](../../data-types.md) | Saved value of the permission to purchase an out-of-stock product: `Y`, `N`, or `D` — the value from the module settings ||
|| **properties**
[`object`](../../data-types.md) | Product property values. The key is `PROPERTY_{id}`, where `id` is the property identifier. The response contains all properties of the information block.

Key value:

- `null` — the value is not filled in
- an object `{"VALUE": ..., "VALUE_ID": ...}` — the value of a single property and the identifier of this value
- an array of such objects — the values of a multiple property

For a "File" type property, `VALUE` returns the file identifier ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Iblock Not Found",
                "field": "200040300000"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `200040300000` | Iblock Not Found | Add the `["iblockId", "=", <identifier>]` condition with an existing information block to `filter` ||
|| `-` | Filter OR groups are not supported | Remove groups with `or` logic from the filter ||
|| `#FIELD#` | DTO `ProductDto` in field `#FIELD#` requires attribute `Filterable` to perform this request | Filter only by the fields from the `filter` parameter description ||
|| `#FIELD#` | DTO `ProductDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Sort only by the fields from the `order` parameter description ||
|| `-` | pagination.offset must be a non-negative multiple of pagination.limit | Pass an `offset` that is a multiple of `limit`, or use `page` ||
|#

#### Pagination Parameter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass a `limit` greater than zero ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `ProductDto` | Specify only the fields from the `select` parameter description. The `PROPERTY_{id}` keys are not supported ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions to view the catalog and to read the information block ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-get-rest-v3.md)
- [{#T}](./catalog-product-add-rest-v3.md)
- [{#T}](./catalog-product-type-list.md)
- [{#T}](./catalog-product-field-list.md)
