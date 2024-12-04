# Get a list of products by filter catalog.product.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of products from the trade catalog based on a filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select** 
[`array`](../../data-types.md)| An array containing the list of fields to select (see fields of the [catalog_product](../data-types.md#catalog_product) object).

Required fields: `id`, `iblockId`
 ||
|| **filter** 
[`object`](../../data-types.md)| An object for filtering the selected products in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_product](../data-types.md#catalog_product) object.

Required fields: `iblockId`.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@` — NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` symbol in the filter value should not be passed. The search looks for a substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol should be passed in the value. Examples:
  - `"mol%"` — searching for values starting with "mol"
  - `"%mol"` — searching for values ending with "mol"
  - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value should not be passed. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol should be passed in the value. Examples:
  - `"mol%"` — searching for values not starting with "mol"
  - `"%mol"` — searching for values not ending with "mol"
  - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
 ||
|| **order**
[`object`](../../data-types.md)| An object for sorting the selected products in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_product](../data-types.md#catalog_product) object.

Possible values for order:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|| **start** 
[`string`](../../data-types.md)| This parameter is used to control pagination.

The page size of results is always static — 50 records.

To select the second page of results, the value `50` must be passed. To select the third page of results, the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number
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
    -d '{"select":["id","iblockId","name","active","available","barcodeMulti","bundle","canBuyZero","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","height","iblockSectionId","length","measure","modifiedBy","previewPicture","previewText","previewTextType","purchasingCurrency","purchasingPrice","quantity","quantityReserved","quantityTrace","recurSchemeLength","recurSchemeType","sort","subscribe","timestampX","trialPriceId","type","vatId","vatIncluded","weight","width","withoutOrder","xmlId","property258","property259"],"filter":{"iblockId":23,">id":10,"@vatId":[1,2]},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","available","barcodeMulti","bundle","canBuyZero","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","height","iblockSectionId","length","measure","modifiedBy","previewPicture","previewText","previewTextType","purchasingCurrency","purchasingPrice","quantity","quantityReserved","quantityTrace","recurSchemeLength","recurSchemeType","sort","subscribe","timestampX","trialPriceId","type","vatId","vatIncluded","weight","width","withoutOrder","xmlId","property258","property259"],"filter":{"iblockId":23,">id":10,"@vatId":[1,2]},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.list
    ```

- JS

    ```js
    BX24.callMethod(
        "catalog.product.list",
        {
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
        'catalog.product.list',
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

- PHP (B24PhpSdk)
  
    ```php        
    try {
        $select = ['id', 'name', 'price', 'active', 'available', 'dateCreate'];
        $filter = ['active' => 'Y'];
        $order = ['name' => 'ASC'];
        $start = 0;
        $result = $serviceBuilder
            ->getCatalogScope()
            ->product()
            ->list($select, $filter, $order, $start);
        foreach ($result->getProducts() as $itemResult) {
            print("ID: {$itemResult->id}\n");
            print("Name: {$itemResult->name}\n");
            print("Active: {$itemResult->active}\n");
            print("Available: {$itemResult->available}\n");
            print("Date Created: {$itemResult->dateCreate->format(DATE_ATOM)}\n");
        }
    } catch (Throwable $e) {
        print("Error: {$e->getMessage()}\n");
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "products": [
            {
                "active": "Y",
                "available": "Y",
                "barcodeMulti": "Y",
                "bundle": "N",
                "canBuyZero": "Y",
                "code": "Product",
                "createdBy": 1,
                "dateActiveFrom": "2024-05-28T10:00:00+02:00",
                "dateActiveTo": "2024-05-29T10:00:00+02:00",
                "dateCreate": "2024-05-27T10:00:00+02:00",
                "detailPicture": {
                    "id": "6439",
                    "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6439\u0026fields%5BproductId%5D=1243",
                    "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6439\u0026fields%5BproductId%5D=1243"
                },
                "detailText": null,
                "detailTextType": "text",
                "height": 100,
                "iblockId": 23,
                "iblockSectionId": 47,
                "id": 1243,
                "length": 100,
                "measure": 5,
                "modifiedBy": 1,
                "name": "Product",
                "previewPicture": {
                    "id": "6438",
                    "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6438\u0026fields%5BproductId%5D=1243",
                    "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6438\u0026fields%5BproductId%5D=1243"
                },
                "previewText": null,
                "previewTextType": "text",
                "property258": {
                    "value": "test",
                    "valueId": "9735"
                },
                "property259": [
                    {
                        "value": "test1",
                        "valueId": "9736"
                    },
                    {
                        "value": "test2",
                        "valueId": "9737"
                    }
                ],
                "purchasingCurrency": "EUR",
                "purchasingPrice": 1000,
                "quantity": 10,
                "quantityReserved": 1,
                "quantityTrace": "Y",
                "recurSchemeLength": 1,
                "recurSchemeType": "D",
                "sort": 100,
                "subscribe": "Y",
                "timestampX": "2024-06-05T10:05:06+02:00",
                "trialPriceId": 175,
                "type": 1,
                "vatId": 1,
                "vatIncluded": "Y",
                "weight": 100,
                "width": 100,
                "withoutOrder": "Y",
                "xmlId": "1243"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1717661048.302644,
        "finish": 1717661049.079089,
        "duration": 0.7764449119567871,
        "processing": 0.3525362014770508,
        "date_start": "2024-06-06T11:04:08+02:00",
        "date_finish": "2024-06-06T11:04:09+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **products**
[`catalog_product[]`](../data-types.md#catalog_product) | An array of objects with information about the selected products ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient rights to read the trade catalog ||
|| `0` | Fields `id`, `iblockId` not specified in the selection fields ||
|| `0` | Field `iblockId` not specified in the filter ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-product-add.md)
- [{#T}](./catalog-product-update.md)
- [{#T}](./catalog-product-get.md)
- [{#T}](./catalog-product-download.md)
- [{#T}](./catalog-product-delete.md)
- [{#T}](./catalog-product-get-fields-by-filter.md)