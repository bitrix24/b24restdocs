# Get a List of Trade Offers catalog.product.offer.list

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method returns a list of trade offers based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | 
An array of fields to select (see fields of the object [catalog_product_offer](../../data-types.md#catalog_product_offer)).

Required fields: `id`, `iblockId`
||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected trade offers in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_offer](../../data-types.md#catalog_product_offer). 

Required fields: `iblockId`.

An additional prefix can be specified for the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal ||
|| **order**
[`object`](../../../data-types.md) | 
An object for sorting the selected trade offers in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_offer](../../data-types.md#catalog_product_offer).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number
||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","available","barcodeMulti","bundle","canBuyZero","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","height","iblockSectionId","length","measure","modifiedBy","previewPicture","previewText","previewTextType","purchasingCurrency","purchasingPrice","quantity","quantityReserved","quantityTrace","recurSchemeLength","recurSchemeType","sort","subscribe","timestampX","trialPriceId","type","vatId","vatIncluded","weight","width","withoutOrder","xmlId","parentId","property258","property259"],"filter":{"iblockId":24,">id":10,"@vatId":[1,2]},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","available","barcodeMulti","bundle","canBuyZero","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","height","iblockSectionId","length","measure","modifiedBy","previewPicture","previewText","previewTextType","purchasingCurrency","purchasingPrice","quantity","quantityReserved","quantityTrace","recurSchemeLength","recurSchemeType","sort","subscribe","timestampX","trialPriceId","type","vatId","vatIncluded","weight","width","withoutOrder","xmlId","parentId","property258","property259"],"filter":{"iblockId":24,">id":10,"@vatId":[1,2]},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.list
    ```

- JS

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
                "@vatId": [1, 2],
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

- PHP

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
                "@vatId" => [1, 2],
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

HTTP Status: **200**

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
                "name": "Trade Offer",
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
[`object`](../../../data-types.md) | Root element of the response ||
|| **offers**
[`catalog_product_offer[]`](../../data-types.md#catalog_product_offer) | An array of objects containing information about the selected trade offers ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

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
|| `200040300010` | Insufficient rights to read the trade catalog
|| 
|| `0` | Fields `id`, `iblockId` not specified in the selection fields
|| 
|| `0` | Field `iblockId` not specified in the filter
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-add.md)
- [{#T}](./catalog-product-offer-update.md)
- [{#T}](./catalog-product-offer-get.md)
- [{#T}](./catalog-product-offer-download.md)
- [{#T}](./catalog-product-offer-delete.md)
- [{#T}](./catalog-product-offer-get-fields-by-filter.md)