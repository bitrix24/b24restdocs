# Get List of Services v 3.0 catalog.product.service.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `catalog.product.service.list` method returns a list of trade catalog services by filter.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | A filter in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`. The filter syntax is described in the [REST 3.0 overview](../../../rest-v3.md#filter).

The `["iblockId", "=", <catalog_id>]` condition is required. The identifier can be retrieved using the [catalog.catalog.list](../../catalog/catalog-catalog-list-rest-v3.md) method.

You can filter by the `id`, `iblockId`, `name`, `code`, `xmlId`, `active`, `sort`, `createdBy`, `modifiedBy`, `dateCreate`, `timestampX`, `dateActiveFrom`, `dateActiveTo`, `iblockSectionId`, `type`, `available`, `bundle`, `quantity`, `quantityReserved`, `quantityTrace`, `canBuyZero`, `subscribe`, `vatId`, `vatIncluded`, `purchasingPrice`, `purchasingCurrency`, `barcodeMulti`, `weight`, `length`, `width`, `height`, and `measure` fields. Property values are not supported in the filter.

The `type` field supports the `=`, `!=`, and `in` operators. The method skips an `=` condition with a type other than `7`. Groups with `or` logic and negation are not supported. An `in` condition with an empty list returns an empty list ||
|| **select**
[`array`](../../../data-types.md) | An array of service fields to return in the response.

Available fields:

- `id` — service identifier
- `iblockId` — information block identifier
- `name` — name
- `code` — symbolic code
- `xmlId` — external code
- `active` — activity status
- `sort` — sort order
- `createdBy` — created by
- `modifiedBy` — modified by
- `dateCreate` — creation date
- `timestampX` — modification date
- `dateActiveFrom` — activity start
- `dateActiveTo` — activity end
- `previewText` — preview description
- `previewTextType` — preview description type
- `previewPicture` — preview picture
- `detailText` — detail description
- `detailTextType` — detail description type
- `detailPicture` — detail picture
- `iblockSectionId` — main section
- `iblockSection` — all sections
- `type` — product type
- `available` — available for purchase
- `bundle` — set flag
- `quantity` — quantity
- `quantityReserved` — reserved quantity
- `quantityTrace` — quantity tracking
- `canBuyZero` — out-of-stock purchase
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
- `trialPriceId` — product for a trial payment
- `withoutOrder` — renewal without an order
- `quantityTraceRaw` — saved tracking mode
- `paymentType` — payment type
- `subscribeRaw` — saved subscription permission
- `canBuyZeroRaw` — saved out-of-stock purchase permission
- `properties` — property values

Property values are returned only as a whole in the `properties` field; individual `PROPERTY_{id}` keys cannot be passed in `select`. The `["*"]` value is not supported. The `id` field is returned only if it is specified in `select`.

Default: all fields ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the `{"field": "direction"}` format, for example `{"id": "desc"}`.

Possible direction values:

- `asc` — ascending
- `desc` — descending

You can sort by the `id`, `iblockId`, `name`, `code`, `xmlId`, `active`, `sort`, `dateCreate`, `timestampX`, `dateActiveFrom`, `dateActiveTo`, `type`, and `quantity` fields ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination. [Parameter description](#pagination) ||
|#

### Parameter Pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of services per page. Values greater than `50` are reduced to `50`. The value `0` causes an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1` ||
|| **offset**
[`integer`](../../../data-types.md) | Number of records to skip. The value must be a multiple of `limit` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.service.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["iblockId","=",25],["active","=","Y"]],"select":["id","name","xmlId","vatId","measure"],"order":{"id":"desc"},"pagination":{"limit":2}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["iblockId","=",25],["active","=","Y"]],"select":["id","name","xmlId","vatId","measure"],"order":{"id":"desc"},"pagination":{"limit":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.service.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ServiceListResult = {
      items: Array<{
        id: number
        name: string
        xmlId: string | null
        vatId: number | null
        measure: number | null
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<ServiceListResult>({
        method: 'catalog.product.service.list',
        params: {
          filter: [
            ['iblockId', '=', 25],
            ['active', '=', 'Y'],
          ],
          select: ['id', 'name', 'xmlId', 'vatId', 'measure'],
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
        console.info('Services:', result.items.length)
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
      async function listServices() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.service.list',
            params: {
              filter: [
                ['iblockId', '=', 25],
                ['active', '=', 'Y'],
              ],
              select: ['id', 'name', 'xmlId', 'vatId', 'measure'],
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
          console.info('Services:', result.items.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listServices)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.service.list(
            filter=[
                ["iblockId", "=", 25],
                ["active", "=", "Y"],
            ],
            select=["id", "name", "xmlId", "vatId", "measure"],
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
                'catalog.product.service.list',
                [
                    'filter' => [
                        ['iblockId', '=', 25],
                        ['active', '=', 'Y'],
                    ],
                    'select' => ['id', 'name', 'xmlId', 'vatId', 'measure'],
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

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.product.service.list',
        {
            filter: [
                ['iblockId', '=', 25],
                ['active', '=', 'Y'],
            ],
            select: ['id', 'name', 'xmlId', 'vatId', 'measure'],
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
        'catalog.product.service.list',
        [
            'filter' => [
                ['iblockId', '=', 25],
                ['active', '=', 'Y'],
            ],
            'select' => ['id', 'name', 'xmlId', 'vatId', 'measure'],
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
    res, err := client.Core().Call(ctx, "catalog.product.service.list", b24.Params{
    	"filter":     []any{
    		[]any{
    			"iblockId",
    			"=",
    			25,
    		},
    		[]string{"active", "=", "Y"},
    	},
    	"select":     []string{"id", "name", "xmlId", "vatId", "measure"},
    	"order":      b24.Params{
    		"id": "desc",
    	},
    	"pagination": b24.Params{
    		"limit": 2,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.service.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID   int    `json:"id"`
    		Name string `json:"name"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Name)
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
                "id": 7551,
                "name": "Equipment installation and setup",
                "xmlId": "DOCS-V3-SERVICE-1",
                "vatId": 1,
                "measure": 9
            },
            {
                "id": 6975,
                "name": "Service",
                "xmlId": "6975",
                "vatId": null,
                "measure": 9
            }
        ]
    },
    "time": {
        "start": 1792660787,
        "finish": 1792660787.672372,
        "duration": 0.6723721027374268,
        "processing": 0,
        "date_start": "2026-10-22T12:19:47+02:00",
        "date_finish": "2026-10-22T12:19:47+02:00",
        "operating_reset_at": 1792661340,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object with the response data ||
|| **result.items**
[`array`](../../../data-types.md) | An array of services. The fields of each service depend on `select`. [Field description](#item). The method does not return the total number of records ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Service identifier ||
|| **iblockId**
[`integer`](../../../data-types.md) | Identifier of the trade catalog information block ||
|| **name**
[`string`](../../../data-types.md) | Name of the service ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../../../data-types.md) | External code. If the code was not passed on creation, it equals the service identifier ||
|| **active**
[`string`](../../../data-types.md) | Service activity status: `Y` — active, `N` — inactive ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the service ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the service ||
|| **dateCreate**
[`datetime`](../../../data-types.md) | Creation date and time ||
|| **timestampX**
[`datetime`](../../../data-types.md) | Date and time of the last modification ||
|| **dateActiveFrom**
[`datetime`](../../../data-types.md) | Activity start date ||
|| **dateActiveTo**
[`datetime`](../../../data-types.md) | Activity end date ||
|| **previewText**
[`string`](../../../data-types.md) | Preview description ||
|| **previewTextType**
[`string`](../../../data-types.md) | Preview description type: `text` or `html` ||
|| **previewPicture**
[`string`](../../../data-types.md) | Preview picture file identifier as a string. `null` if there is no picture ||
|| **detailText**
[`string`](../../../data-types.md) | Detail description ||
|| **detailTextType**
[`string`](../../../data-types.md) | Detail description type: `text` or `html` ||
|| **detailPicture**
[`string`](../../../data-types.md) | Detail picture file identifier as a string. `null` if there is no picture. The file can be retrieved using the [catalog.product.service.download](./catalog-product-service-download-rest-v3.md) method ||
|| **iblockSectionId**
[`integer`](../../../data-types.md) | Main section identifier. `null` if the service is not linked to a section ||
|| **iblockSection**
[`array`](../../../data-types.md) | An array of identifiers of all sections the service is linked to ||
|| **type**
[`integer`](../../../data-types.md) | Product type. Always `7` for a service ||
|| **available**
[`string`](../../../data-types.md) | Available for purchase: `Y` — yes, `N` — no ||
|| **bundle**
[`string`](../../../data-types.md) | Set flag: `Y` — yes, `N` — no ||
|| **quantity**
[`double`](../../../data-types.md) | Quantity ||
|| **quantityReserved**
[`double`](../../../data-types.md) | Reserved quantity ||
|| **quantityTrace**
[`string`](../../../data-types.md) | Effective quantity tracking mode, taking the catalog settings into account: `Y` — enabled, `N` — disabled ||
|| **canBuyZero**
[`string`](../../../data-types.md) | Effective out-of-stock purchase permission, taking the catalog settings into account: `Y` — yes, `N` — no ||
|| **subscribe**
[`string`](../../../data-types.md) | Effective subscription permission, taking the catalog settings into account: `Y` — yes, `N` — no ||
|| **vatId**
[`integer`](../../../data-types.md) | VAT rate identifier. `null` if no rate is set ||
|| **vatIncluded**
[`string`](../../../data-types.md) | VAT is included in the price: `Y` — yes, `N` — no ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchase price. `null` if not set ||
|| **purchasingCurrency**
[`string`](../../../data-types.md) | Purchase price currency. `null` if not set ||
|| **barcodeMulti**
[`string`](../../../data-types.md) | Unique barcodes for each item: `Y` — yes, `N` — no ||
|| **weight**
[`double`](../../../data-types.md) | Weight ||
|| **length**
[`double`](../../../data-types.md) | Length. `null` if not set ||
|| **width**
[`double`](../../../data-types.md) | Width. `null` if not set ||
|| **height**
[`double`](../../../data-types.md) | Height. `null` if not set ||
|| **measure**
[`integer`](../../../data-types.md) | Unit of measurement identifier ||
|| **recurSchemeLength**
[`integer`](../../../data-types.md) | Payment period length. Used for content sales in the on-premise version ||
|| **recurSchemeType**
[`string`](../../../data-types.md) | Time unit of the payment period, for example `D` — day. Used for content sales in the on-premise version ||
|| **trialPriceId**
[`integer`](../../../data-types.md) | Identifier of the product for a trial payment. Used for content sales in the on-premise version ||
|| **withoutOrder**
[`string`](../../../data-types.md) | Renewal without placing an order: `Y` — yes, `N` — no ||
|| **quantityTraceRaw**
[`string`](../../../data-types.md) | Saved value of the quantity tracking mode: `Y`, `N`, or `D` — according to the catalog settings ||
|| **paymentType**
[`string`](../../../data-types.md) | Payment type: `S` — one-time, `R` — recurring, `T` — trial ||
|| **subscribeRaw**
[`string`](../../../data-types.md) | Saved value of the subscription permission: `Y`, `N`, or `D` — according to the catalog settings ||
|| **canBuyZeroRaw**
[`string`](../../../data-types.md) | Saved value of the out-of-stock purchase permission: `Y`, `N`, or `D` — according to the catalog settings ||
|| **properties**
[`object`](../../../data-types.md) | Service property values. The keys are `PROPERTY_{id}`, where `{id}` is the property identifier. A property value is an object `{"VALUE": "...", "VALUE_ID": "..."}`, and for a multiple property it is an array of such objects. `null` if no value is set ||
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

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `200040300000` | Iblock Not Found | Pass the `["iblockId", "=", <catalog_id>]` condition with an existing trade catalog in `filter` ||
|| `-` | productType is not allowed for this catalog | An offers catalog contains no services. Pass the identifier of a product catalog ||
|| `-` | The type field supports only the =, != and in filter operators | Use the `=`, `!=`, or `in` operators for `type` ||
|| `-` | Filter OR groups are not supported | Remove groups with `or` logic from the filter ||
|| `-` | pagination.offset must be a non-negative multiple of pagination.limit | Pass an `offset` that is a multiple of `limit`, or use `page` ||
|| `#FIELD#` | DTO `ProductDto` in field `#FIELD#` requires attribute `Filterable` to perform this request | Filter only by the fields from the `filter` parameter description ||
|| `#FIELD#` | DTO `ProductDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Sort only by the fields from the `order` parameter description ||
|#

#### Filter Operator Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `#OPERATOR#` | Use the operators from the [REST 3.0 overview](../../../rest-v3.md#operators) ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass a number greater than `0` in `limit` ||
|#

#### Errors in the `select`, `filter`, and `order` Parameters

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductDto` | Specify only the fields from the `select` parameter description. `PROPERTY_{id}` keys are not supported ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions to view the catalog and read information block elements ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-get-rest-v3.md)
- [{#T}](./catalog-product-service-add-rest-v3.md)
- [{#T}](./catalog-product-service-field-list.md)
- [{#T}](../../catalog/catalog-catalog-list-rest-v3.md)
