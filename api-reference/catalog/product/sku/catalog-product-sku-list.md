# Get a List of Parent Products catalog.product.sku.list

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method returns a list of parent products based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | 
An array of fields to select (see fields of the object [catalog_product_sku](../../data-types.md#catalog_product_sku)).

Required fields: `id`, `iblockId`
||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected parent products in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_sku](../../data-types.md#catalog_product_sku). 

Required fields: `iblockId`.

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character should not be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` character should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` character should not be included in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` character should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equals, exact match (used by default)
- `!=` — not equal
- `!` — not equal ||
|| **order**
[`object`](../../../data-types.md) | 
An object for sorting the selected parent products in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_sku](../../data-types.md#catalog_product_sku).

Possible values for `order`:
- `asc` — ascending order
- `desc` — descending order
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","available","bundle","canBuyZero","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","height","iblockSectionId","length","measure","modifiedBy","previewPicture","previewText","previewTextType","purchasingCurrency","purchasingPrice","quantity","sort","subscribe","timestampX","type","vatId","vatIncluded","weight","width","xmlId","property258","property259"],"filter":{"iblockId":23,">id":10,"@vatId":[1,2]},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","available","bundle","canBuyZero","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","height","iblockSectionId","length","measure","modifiedBy","previewPicture","previewText","previewTextType","purchasingCurrency","purchasingPrice","quantity","sort","subscribe","timestampX","type","vatId","vatIncluded","weight","width","xmlId","property258","property259"],"filter":{"iblockId":23,">id":10,"@vatId":[1,2]},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.sku.list
    ```

- JS

    ```js
    BX24.callMethod(
        "catalog.product.sku.list", {
            "select": [
                "id",
                "iblockId",
                "name",
                "active",
                "available",
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
                "sort",
                "subscribe",
                "timestampX",
                "type",
                "vatId",
                "vatIncluded",
                "weight",
                "width",
                "xmlId",
                "property258",
                "property259",
            ],
            "filter": {
                "iblockId": 23,
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
        'catalog.product.sku.list',
        [
            'select' => [
                "id",
                "iblockId",
                "name",
                "active",
                "available",
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
                "sort",
                "subscribe",
                "timestampX",
                "type",
                "vatId",
                "vatIncluded",
                "weight",
                "width",
                "xmlId",
                "property258",
                "property259",
            ],
            'filter' => [
                "iblockId" => 23,
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

HTTP status: **200**

```json
{
    "result": {
        "units": [
            {
                "active": "Y",
                "available": "N",
                "bundle": "N",
                "canBuyZero": "N",
                "code": "product_sku",
                "createdBy": 1,
                "dateActiveFrom": "2024-05-28T10:00:00+03:00",
                "dateActiveTo": "2024-05-29T10:00:00+03:00",
                "dateCreate": "2024-05-27T10:00:00+03:00",
                "detailPicture": {
                    "id": "6546",
                    "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6546\u0026fields%5BproductId%5D=1289",
                    "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6546\u0026fields%5BproductId%5D=1289"
                },
                "detailText": null,
                "detailTextType": "text",
                "height": null,
                "iblockId": 23,
                "iblockSectionId": 47,
                "id": 1289,
                "length": null,
                "measure": null,
                "modifiedBy": 1,
                "name": "Parent Product",
                "previewPicture": {
                    "id": "6545",
                    "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6545\u0026fields%5BproductId%5D=1289",
                    "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6545\u0026fields%5BproductId%5D=1289"
                },
                "previewText": null,
                "previewTextType": "text",
                "property258": {
                    "value": "test",
                    "valueId": "9877"
                },
                "property259": [
                    {
                        "value": "test1",
                        "valueId": "9878"
                    },
                    {
                        "value": "test2",
                        "valueId": "9879"
                    }
                ],
                "purchasingCurrency": null,
                "purchasingPrice": null,
                "quantity": null,
                "sort": 100,
                "subscribe": "Y",
                "timestampX": "2024-06-17T14:30:33+03:00",
                "type": 6,
                "vatId": null,
                "vatIncluded": "N",
                "weight": null,
                "width": null,
                "xmlId": "1289"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1718631434.15105,
        "finish": 1718631434.921837,
        "duration": 0.7707870006561279,
        "processing": 0.3575417995452881,
        "date_start": "2024-06-17T16:37:14+03:00",
        "date_finish": "2024-06-17T16:37:14+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **units**
[`catalog_product_sku[]`](../../data-types.md#catalog_product_sku) | An array of objects with information about the selected parent products ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
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
|| `200040300010` | Insufficient permissions to read the trade catalog
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

- [{#T}](./catalog-product-sku-add.md)
- [{#T}](./catalog-product-sku-update.md)
- [{#T}](./catalog-product-sku-get.md)
- [{#T}](./catalog-product-sku-download.md)
- [{#T}](./catalog-product-sku-delete.md)
- [{#T}](./catalog-product-sku-get-fields-by-filter.md)