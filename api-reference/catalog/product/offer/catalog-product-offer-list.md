# Get a list of product variations catalog.product.offer.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of product variations based on a filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | 
An array containing a list of fields that must be selected (see the fields of the [catalog_product_offer](../../data-types.md#catalog_product_offer) object).

Required fields: `id`, `iblockId`
||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering selected product variations in `{"field_1": "value_1", ... "field_N": "value_N"}` format.

Possible values for `field` correspond to the fields of the [catalog_product_offer](../../data-types.md#catalog_product_offer) object. 

Required fields: `iblockId` — the identifier of the trade catalog information block for variations. 
To obtain existing identifiers of trade catalog information blocks, you must use [catalog.catalog.list](../../catalog/catalog-catalog-list.md). The variations information block has the `productIblockId` field populated.

A prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` character does not need to be passed in the filter value. The search looks for a substring in any position of the string.
- `=%` — LIKE, substring search. The `%` character must be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` character does not need to be passed in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` character must be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the "mol" substring is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default). For IN searches, multiple values can be passed as an array.
- `!=` — not equal
- `!` — not equal. For NOT IN searches, multiple values can be passed as an array.

For `propertyN` properties, the value format in `filter` depends on the condition type:
- if you are filtering a date or datetime type property by range, use the prefix in the key and pass the value in the `value` field
- if you are filtering a numeric, string, or list property by exact match, pass the value directly without `value`
- for `IN` and NOT IN searches, an array of values can be passed.

Examples of filtering by properties:

```js
filter: {
    iblockId: 24,
    '>=property424': {
        value: '2025-05-29T12:00:00+03:00'
    },
    '<=property424': {
        value: '2025-05-30T13:00:00+03:00'
    },
    property996: 9636,
    property997: [9636, 568, 570, 9658],
}
```
||
|| **order**
[`object`](../../../data-types.md) | 
An object for sorting selected product variations in `{"field_1": "order_1", ... "field_N": "order_N"}` format.

Possible values for `field` correspond to the fields of the [catalog_product_offer](../../data-types.md#catalog_product_offer) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../../data-types.md) | The parameter is used to control pagination.

The results page size is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100` and so on.

Formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the required page
||
|#

{% note warning "Working with variation prices" %}

To retrieve variation prices, use the [catalog.price.*](../../price/index.md) methods.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","available","barcodeMulti","bundle","canBuyZero","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","height","iblockSectionId","length","measure","modifiedBy","previewPicture","previewText","previewTextType","purchasingCurrency","purchasingPrice","quantity","quantityReserved","quantityTrace","recurSchemeLength","recurSchemeType","sort","subscribe","timestampX","trialPriceId","type","vatId","vatIncluded","weight","width","withoutOrder","xmlId","parentId","property258","property259"],"filter":{"iblockId":24,">id":10,"vatId":[1,2]},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","available","barcodeMulti","bundle","canBuyZero","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","height","iblockSectionId","length","measure","modifiedBy","previewPicture","previewText","previewTextType","purchasingCurrency","purchasingPrice","quantity","quantityReserved","quantityTrace","recurSchemeLength","recurSchemeType","sort","subscribe","timestampX","trialPriceId","type","vatId","vatIncluded","weight","width","withoutOrder","xmlId","parentId","property258","property259"],"filter":{"iblockId":24,">id":10,"vatId":[1,2]},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductOffer = {
      id: number
      iblockId: number
      name: string
      active: string
      available: string
      barcodeMulti: string
      bundle: string
      canBuyZero: string
      code: string
      createdBy: number
      dateActiveFrom: ISODate | null
      dateActiveTo: ISODate | null
      dateCreate: ISODate | null
      detailPicture: { id: string; url: string; urlMachine: string } | null
      detailText: string | null
      detailTextType: string
      height: number
      iblockSectionId: number | null
      length: number
      measure: number
      modifiedBy: number
      parentId: { value: string; valueId: string }
      previewPicture: { id: string; url: string; urlMachine: string } | null
      previewText: string | null
      previewTextType: string
      purchasingCurrency: string
      purchasingPrice: number
      quantity: number
      quantityReserved: number
      quantityTrace: string
      recurSchemeLength: number | null
      recurSchemeType: string
      sort: number
      subscribe: string
      timestampX: ISODate | null
      trialPriceId: number | null
      type: number
      vatId: number
      vatIncluded: string
      weight: number
      width: number
      withoutOrder: string
      xmlId: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductOfferListResult = {
      offers: ProductOffer[]
    }

    try {
      // catalog.product.offer.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ProductOfferListResult>({
        method: 'catalog.product.offer.list',
        params: {
          select: [
            'id',
            'iblockId',
            'name',
            'active',
            'available',
            'barcodeMulti',
            'bundle',
            'canBuyZero',
            'code',
            'createdBy',
            'dateActiveFrom',
            'dateActiveTo',
            'dateCreate',
            'detailPicture',
            'detailText',
            'detailTextType',
            'height',
            'iblockSectionId',
            'length',
            'measure',
            'modifiedBy',
            'previewPicture',
            'previewText',
            'previewTextType',
            'purchasingCurrency',
            'purchasingPrice',
            'quantity',
            'quantityReserved',
            'quantityTrace',
            'recurSchemeLength',
            'recurSchemeType',
            'sort',
            'subscribe',
            'timestampX',
            'trialPriceId',
            'type',
            'vatId',
            'vatIncluded',
            'weight',
            'width',
            'withoutOrder',
            'xmlId',
            'parentId',
            'property258',
            'property259',
          ],
          filter: {
            iblockId: 24,
            '>id': 10,
            vatId: [1, 2],
          },
          order: {
            id: 'desc',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Offers count:', result.offers.length, 'First offer:', result.offers[0])
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
      async function listProductOffers() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.product.offer.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.offer.list',
            params: {
              select: [
                'id',
                'iblockId',
                'name',
                'active',
                'available',
                'barcodeMulti',
                'bundle',
                'canBuyZero',
                'code',
                'createdBy',
                'dateActiveFrom',
                'dateActiveTo',
                'dateCreate',
                'detailPicture',
                'detailText',
                'detailTextType',
                'height',
                'iblockSectionId',
                'length',
                'measure',
                'modifiedBy',
                'previewPicture',
                'previewText',
                'previewTextType',
                'purchasingCurrency',
                'purchasingPrice',
                'quantity',
                'quantityReserved',
                'quantityTrace',
                'recurSchemeLength',
                'recurSchemeType',
                'sort',
                'subscribe',
                'timestampX',
                'trialPriceId',
                'type',
                'vatId',
                'vatIncluded',
                'weight',
                'width',
                'withoutOrder',
                'xmlId',
                'parentId',
                'property258',
                'property259',
              ],
              filter: {
                iblockId: 24,
                '>id': 10,
                vatId: [1, 2],
              },
              order: {
                id: 'desc',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Offers count:', result.offers.length, 'First offer:', result.offers[0])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listProductOffers)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.product.offer.list',
                [
                    'select' => [
                        'id',
                        'iblockId',
                        'name',
                        'active',
                        'available',
                        'barcodeMulti',
                        'bundle',
                        'canBuyZero',
                        'code',
                        'createdBy',
                        'dateActiveFrom',
                        'dateActiveTo',
                        'dateCreate',
                        'detailPicture',
                        'detailText',
                        'detailTextType',
                        'height',
                        'iblockSectionId',
                        'length',
                        'measure',
                        'modifiedBy',
                        'previewPicture',
                        'previewText',
                        'previewTextType',
                        'purchasingCurrency',
                        'purchasingPrice',
                        'quantity',
                        'quantityReserved',
                        'quantityTrace',
                        'recurSchemeLength',
                        'recurSchemeType',
                        'sort',
                        'subscribe',
                        'timestampX',
                        'trialPriceId',
                        'type',
                        'vatId',
                        'vatIncluded',
                        'weight',
                        'width',
                        'withoutOrder',
                        'xmlId',
                        'parentId',
                        'property258',
                        'property259',
                    ],
                    'filter' => [
                        'iblockId' => 24,
                        '>id'      => 10,
                        'vatId'    => [1, 2],
                    ],
                    'order'  => [
                        'id' => 'desc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching product offers: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "catalog.product.offer.list", {
            "select": [
                "id",
                "iblockId",
                "name",
                "active",
                "available",
                "barcodeMulti",
                "bundle",
                "canBuyZero",
                "code",
                "createdBy",
                "dateActiveFrom",
                "dateActiveTo",
                "dateCreate",
                "detailPicture",
                "detailText",
                "detailTextType",
                "height",
                "iblockSectionId",
                "length",
                "measure",
                "modifiedBy",
                "previewPicture",
                "previewText",
                "previewTextType",
                "purchasingCurrency",
                "purchasingPrice",
                "quantity",
                "quantityReserved",
                "quantityTrace",
                "recurSchemeLength",
                "recurSchemeType",
                "sort",
                "subscribe",
                "timestampX",
                "trialPriceId",
                "type",
                "vatId",
                "vatIncluded",
                "weight",
                "width",
                "withoutOrder",
                "xmlId",
                "parentId",
                "property258",
                "property259",
            ],
            "filter": {
                "iblockId": 24,
                ">id": 10,
                "vatId": [1, 2],
            },
            "order": {
                "id": "desc",
            }
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
        'catalog.product.offer.list',
        [
            'select' => [
                "id",
                "iblockId",
                "name",
                "active",
                "available",
                "barcodeMulti",
                "bundle",
                "canBuyZero",
                "code",
                "createdBy",
                "dateActiveFrom",
                "dateActiveTo",
                "dateCreate",
                "detailPicture",
                "detailText",
                "detailTextType",
                "height",
                "iblockSectionId",
                "length",
                "measure",
                "modifiedBy",
                "previewPicture",
                "previewText",
                "previewTextType",
                "purchasingCurrency",
                "purchasingPrice",
                "quantity",
                "quantityReserved",
                "quantityTrace",
                "recurSchemeLength",
                "recurSchemeType",
                "sort",
                "subscribe",
                "timestampX",
                "trialPriceId",
                "type",
                "vatId",
                "vatIncluded",
                "weight",
                "width",
                "withoutOrder",
                "xmlId",
                "parentId",
                "property258",
                "property259",
            ],
            'filter' => [
                "iblockId" => 24,
                ">id" => 10,
                "vatId" => [1, 2],
            ],
            'order' => [
                "id" => "desc",
            ]
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
        "offers": [
            {
                "active": "Y",
                "available": "Y",
                "barcodeMulti": "N",
                "bundle": "N",
                "canBuyZero": "Y",
                "code": "Product",
                "createdBy": 1,
                "dateActiveFrom": "2024-05-28T10:00:00+03:00",
                "dateActiveTo": "2024-05-29T10:00:00+03:00",
                "dateCreate": "2024-05-27T10:00:00+03:00",
                "detailPicture": {
                    "id": "6538",
                    "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6538\u0026fields%5BproductId%5D=1286",
                    "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6538\u0026fields%5BproductId%5D=1286"
                },
                "detailText": null,
                "detailTextType": "text",
                "height": 100,
                "iblockId": 24,
                "iblockSectionId": null,
                "id": 1286,
                "length": 100,
                "measure": 5,
                "modifiedBy": 1,
                "name": "Product variation",
                "parentId": {
                    "value": "1275",
                    "valueId": "9867"
                },
                "previewPicture": {
                    "id": "6537",
                    "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6537\u0026fields%5BproductId%5D=1286",
                    "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6537\u0026fields%5BproductId%5D=1286"
                },
                "previewText": null,
                "previewTextType": "text",
                "purchasingCurrency": "USD",
                "purchasingPrice": 1000,
                "quantity": 10,
                "quantityReserved": 1,
                "quantityTrace": "Y",
                "recurSchemeLength": null,
                "recurSchemeType": "D",
                "sort": 100,
                "subscribe": "Y",
                "timestampX": "2024-06-17T12:29:59+03:00",
                "trialPriceId": null,
                "type": 4,
                "vatId": 1,
                "vatIncluded": "Y",
                "weight": 100,
                "width": 100,
                "withoutOrder": "N",
                "xmlId": "1286"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1718625858.291581,
        "finish": 1718625858.937026,
        "duration": 0.6454451084136963,
        "processing": 0.2336890697479248,
        "date_start": "2024-06-17T15:04:18+03:00",
        "date_finish": "2024-06-17T15:04:18+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root response item ||
|| **offers**
[`catalog_product_offer[]`](../../data-types.md#catalog_product_offer) | An array of objects containing information about the selected product variations ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read the trade catalog ||
|| `0` | Fields `id`, `iblockId` are not specified in the selection fields ||
|| `0` | Field `iblockId` is not specified in the filter ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-add.md)
- [{#T}](./catalog-product-offer-update.md)
- [{#T}](./catalog-product-offer-get.md)
- [{#T}](./catalog-product-offer-download.md)
- [{#T}](./catalog-product-offer-delete.md)
- [{#T}](./catalog-product-offer-get-fields-by-filter.md)
