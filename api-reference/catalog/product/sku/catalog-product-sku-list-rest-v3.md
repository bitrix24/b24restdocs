# Get List of Parent Products v 3.0 catalog.product.sku.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `catalog.product.sku.list` returns a list of catalog parent products by filter.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Filter in REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`. The filter syntax is described in the [REST 3.0 overview](../../../rest-v3.md#filter).

You must pass the `["iblockId", "=", 25]` condition with the identifier of the product catalog information block. The identifier can be retrieved using the [catalog.catalog.list](../../catalog/catalog-catalog-list-rest-v3.md) method: it is the catalog whose `productIblockId` equals `0`.

You can filter by the fields: `id`, `iblockId`, `name`, `code`, `xmlId`, `active`, `sort`, `createdBy`, `modifiedBy`, `dateCreate`, `timestampX`, `dateActiveFrom`, `dateActiveTo`, `iblockSectionId`, `type`, `available`, `bundle`, `quantity`, `quantityReserved`, `quantityTrace`, `canBuyZero`, `subscribe`, `vatId`, `vatIncluded`, `purchasingPrice`, `purchasingCurrency`, `barcodeMulti`, `weight`, `length`, `width`, `height`, `measure`. Filtering by properties is not supported.

The `=`, `!=`, `>`, `>=`, `<`, and `<=` operators are supported. The `in` operator is not currently applied: the method returns records without taking this condition into account. The `%` operator, `or` logic, and negation are not supported.

The `type` field supports the `=`, `!=`, and `in` operators. A value other than `3` or `6` does not narrow the selection.

If you pass two conditions with the same operator for one field, the last one is applied ||
|| **select**
[`array`](../../../data-types.md) | Array of parent product fields to return in the response.

Available fields:

- `id` — identifier
- `iblockId` — information block identifier
- `name` — name
- `code` — symbolic code
- `xmlId` — external code
- `active` — activity
- `sort` — sort index
- `createdBy` — created by
- `modifiedBy` — modified by
- `dateCreate` — creation date
- `timestampX` — modification date
- `dateActiveFrom` — activity start
- `dateActiveTo` — activity end
- `previewText` — preview description
- `previewTextType` — preview description type
- `previewPicture` — preview picture
- `detailText` — detailed description
- `detailTextType` — detailed description type
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
- `subscribe` — subscription
- `vatId` — VAT rate
- `vatIncluded` — VAT included in the price
- `purchasingPrice` — purchase price
- `purchasingCurrency` — purchase price currency
- `barcodeMulti` — unique barcodes
- `weight` — weight
- `length` — length
- `width` — width
- `height` — height
- `measure` — unit of measurement
- `recurSchemeLength` — payment period length
- `recurSchemeType` — payment period unit
- `trialPriceId` — product for trial payment
- `withoutOrder` — renewal without an order
- `quantityTraceRaw` — saved quantity tracking value
- `paymentType` — payment type
- `subscribeRaw` — saved subscription value
- `canBuyZeroRaw` — saved value of purchase when out of stock
- `properties` — property values

Specify `id` in `select`: without it, the method currently returns the `INTERNAL_SERVER_ERROR` error with HTTP status `500`. The `["*"]` and `PROPERTY_{id}` values are not supported: property values are returned by the `properties` field.

Default: all fields except `properties`. To retrieve property values, pass `properties` in `select` ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the `{"field": "direction"}` format, for example `{"id": "desc"}`.

Possible direction values:

- `asc` — ascending
- `desc` — descending

You can sort by the fields: `id`, `iblockId`, `name`, `code`, `xmlId`, `active`, `sort`, `dateCreate`, `timestampX`, `dateActiveFrom`, `dateActiveTo`, `type`, `quantity` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination. [Object description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of records per page. Values greater than `50` are reduced to `50`. The `0` value causes an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1` ||
|| **offset**
[`integer`](../../../data-types.md) | Offset from the beginning of the list. Must be a multiple of `limit` ||
|#

{% note warning "" %}

A page number beyond the list returns the first page, not an empty array. Do not end the iteration on an empty page: the loop will not stop. If a page has fewer records than `limit`, it is the last page. If the number of records is a multiple of `limit`, the next page repeats the first one — compare the identifiers.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.sku.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["iblockId","=",25],["iblockSectionId","=",31],["type","=",3]],"select":["id","name","type","active"],"order":{"id":"desc"},"pagination":{"limit":2}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["iblockId","=",25],["iblockSectionId","=",31],["type","=",3]],"select":["id","name","type","active"],"order":{"id":"desc"},"pagination":{"limit":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.sku.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SkuListResult = {
      items: Array<{
        id: number
        name: string
        type: number
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<SkuListResult>({
        method: 'catalog.product.sku.list',
        params: {
          filter: [
            ['iblockId', '=', 25],
            ['iblockSectionId', '=', 31],
            ['type', '=', 3],
          ],
          select: ['id', 'name', 'type', 'active'],
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
        console.info('Items:', result.items.length)
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
      async function listSku() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.sku.list',
            params: {
              filter: [
                ['iblockId', '=', 25],
                ['iblockSectionId', '=', 31],
                ['type', '=', 3],
              ],
              select: ['id', 'name', 'type', 'active'],
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
          console.info('Items:', result.items.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listSku)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.sku.list(
            filter=[
                ["iblockId", "=", 25],
                ["iblockSectionId", "=", 31],
                ["type", "=", 3],
            ],
            select=["id", "name", "type", "active"],
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
                'catalog.product.sku.list',
                [
                    'filter' => [
                        ['iblockId', '=', 25],
                        ['iblockSectionId', '=', 31],
                        ['type', '=', 3],
                    ],
                    'select' => ['id', 'name', 'type', 'active'],
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
        'catalog.product.sku.list',
        {
            filter: [
                ['iblockId', '=', 25],
                ['iblockSectionId', '=', 31],
                ['type', '=', 3],
            ],
            select: ['id', 'name', 'type', 'active'],
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
        'catalog.product.sku.list',
        [
            'filter' => [
                ['iblockId', '=', 25],
                ['iblockSectionId', '=', 31],
                ['type', '=', 3],
            ],
            'select' => ['id', 'name', 'type', 'active'],
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
    res, err := client.Core().Call(ctx, "catalog.product.sku.list", b24.Params{
    	"filter": []any{
    		[]any{"iblockId", "=", 25},
    		[]any{"iblockSectionId", "=", 31},
    		[]any{"type", "=", 3},
    	},
    	"select": []string{"id", "name", "type", "active"},
    	"order": b24.Params{
    		"id": "desc",
    	},
    	"pagination": b24.Params{
    		"limit": 2,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.sku.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID   int    `json:"id"`
    		Name string `json:"name"`
    		Type int    `json:"type"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
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
                "id": 7553,
                "name": "DOCS-V3-SKU Cotton T-shirt",
                "active": "Y",
                "type": 3
            }
        ]
    },
    "time": {
        "start": 1792660767,
        "finish": 1792660767.794007,
        "duration": 0.7940070629119873,
        "processing": 0,
        "date_start": "2026-10-22T12:19:27+02:00",
        "date_finish": "2026-10-22T12:19:27+02:00",
        "operating_reset_at": 1792660860,
        "operating": 0.10251188278198242
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../../../data-types.md) | Array of parent products. The set of fields depends on `select`. [Field description](#items) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Parent product identifier ||
|| **iblockId**
[`integer`](../../../data-types.md) | Identifier of the product catalog information block ||
|| **name**
[`string`](../../../data-types.md) | Parent product name ||
|| **code**
[`string`](../../../data-types.md) or `null` | Symbolic code ||
|| **xmlId**
[`string`](../../../data-types.md) | External code. If not set, it matches `id` ||
|| **active**
[`string`](../../../data-types.md) | Activity: `Y` — active, `N` — inactive ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the record ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the record ||
|| **dateCreate**
[`datetime`](../../../data-types.md) | Creation date and time ||
|| **timestampX**
[`datetime`](../../../data-types.md) | Date and time of the last modification ||
|| **dateActiveFrom**
[`datetime`](../../../data-types.md) or `null` | Activity start date ||
|| **dateActiveTo**
[`datetime`](../../../data-types.md) or `null` | Activity end date ||
|| **previewText**
[`string`](../../../data-types.md) or `null` | Preview description ||
|| **previewTextType**
[`string`](../../../data-types.md) | Preview description type: `text` or `html` ||
|| **previewPicture**
[`string`](../../../data-types.md) or `null` | Preview picture file identifier ||
|| **detailText**
[`string`](../../../data-types.md) or `null` | Detailed description ||
|| **detailTextType**
[`string`](../../../data-types.md) | Detailed description type: `text` or `html` ||
|| **detailPicture**
[`string`](../../../data-types.md) or `null` | Detail picture file identifier ||
|| **iblockSectionId**
[`integer`](../../../data-types.md) or `null` | Main section identifier ||
|| **iblockSection**
[`array`](../../../data-types.md) | Array of identifiers of all sections the product is linked to ||
|| **type**
[`integer`](../../../data-types.md) | Product type: `3` — parent product with variations, `6` — without variations ||
|| **available**
[`string`](../../../data-types.md) | Availability for purchase: `Y` — available, `N` — unavailable ||
|| **bundle**
[`string`](../../../data-types.md) | Whether the product has a bundle: `Y` — yes, `N` — no ||
|| **quantity**
[`double`](../../../data-types.md) | Available quantity ||
|| **quantityReserved**
[`double`](../../../data-types.md) | Reserved quantity ||
|| **quantityTrace**
[`string`](../../../data-types.md) | Whether quantity tracking is enabled: `Y` — yes, `N` — no ||
|| **canBuyZero**
[`string`](../../../data-types.md) | Whether purchase is allowed when the product is out of stock: `Y` — yes, `N` — no ||
|| **subscribe**
[`string`](../../../data-types.md) | Whether subscription to the product is allowed: `Y` — yes, `N` — no ||
|| **vatId**
[`integer`](../../../data-types.md) or `null` | VAT rate identifier ||
|| **vatIncluded**
[`string`](../../../data-types.md) | Whether VAT is included in the price: `Y` — yes, `N` — no ||
|| **purchasingPrice**
[`double`](../../../data-types.md) or `null` | Purchase price. Returned if the user has the "View purchase price" access permission ||
|| **purchasingCurrency**
[`string`](../../../data-types.md) or `null` | Purchase price currency. Returned if the user has the "View purchase price" access permission ||
|| **barcodeMulti**
[`string`](../../../data-types.md) | Unique barcodes for each item: `Y` — yes, `N` — no ||
|| **weight**
[`double`](../../../data-types.md) or `null` | Weight ||
|| **length**
[`double`](../../../data-types.md) or `null` | Length ||
|| **width**
[`double`](../../../data-types.md) or `null` | Width ||
|| **height**
[`double`](../../../data-types.md) or `null` | Height ||
|| **measure**
[`integer`](../../../data-types.md) or `null` | Unit of measurement identifier ||
|| **recurSchemeLength**
[`integer`](../../../data-types.md) or `null` | Payment period length. Used only in the on-premise version for content sales ||
|| **recurSchemeType**
[`string`](../../../data-types.md) | Time unit of the payment period. Used only in the on-premise version for content sales ||
|| **trialPriceId**
[`integer`](../../../data-types.md) or `null` | Product for trial payment. Used only in the on-premise version for content sales ||
|| **withoutOrder**
[`string`](../../../data-types.md) | Renewal without placing an order: `Y` — yes, `N` — no. Used only in the on-premise version for content sales ||
|| **quantityTraceRaw**
[`string`](../../../data-types.md) | Saved quantity tracking value: `Y` — yes, `N` — no, `D` — according to the module settings ||
|| **paymentType**
[`string`](../../../data-types.md) | Payment type: `S` — one-time, `R` — recurring, `T` — trial. Used only in the on-premise version for content sales ||
|| **subscribeRaw**
[`string`](../../../data-types.md) | Saved subscription value: `Y` — yes, `N` — no, `D` — according to the module settings ||
|| **canBuyZeroRaw**
[`string`](../../../data-types.md) | Saved value of purchase when out of stock: `Y` — yes, `N` — no, `D` — according to the module settings ||
|| **properties**
[`object`](../../../data-types.md) | Product property values. The keys are `PROPERTY_{id}`, where `{id}` is the property identifier. The value is `null`, a `{"VALUE": ..., "VALUE_ID": ...}` object, or an array of such objects for a multiple property ||
|#

The total number of records is not returned in the response.

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

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `200040300000` | Iblock Not Found | Pass the `iblockId` condition with the identifier of an existing catalog in `filter` ||
|| `-` | productType is not allowed for this catalog | Pass the product catalog information block in `iblockId` ||
|| `-` | The type field supports only the =, != and in filter operators | Use the `=`, `!=`, or `in` operators for `type` ||
|| `#FIELD#` | DTO `ProductDto` in field `#FIELD#` requires attribute `Filterable` to perform this request | Filter only by the fields from the `filter` parameter description ||
|| `#FIELD#` | DTO `ProductDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Sort only by the fields from the `order` parameter description ||
|| `-` | pagination.offset must be a non-negative multiple of pagination.limit | Pass an `offset` that is a multiple of `limit`, or use `page` ||
|#

#### Filter Operator Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `#OPERATOR#` | Use the `=`, `!=`, `>`, `>=`, `<`, `<=` operators ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass a `limit` greater than `0` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `ProductDto` | Specify only the fields from the parameter descriptions. `PROPERTY_{id}` keys are not supported ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions for the catalog and the information block ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-sku-add-rest-v3.md)
- [{#T}](./catalog-product-sku-update-rest-v3.md)
- [{#T}](./catalog-product-sku-get-rest-v3.md)
- [{#T}](./catalog-product-sku-delete-rest-v3.md)
- [{#T}](./catalog-product-sku-download-rest-v3.md)
- [{#T}](./catalog-product-sku-field-list.md)
- [{#T}](./catalog-product-sku-field-get.md)
