# Set Products in Deal crm.deal.productrows.set

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with "modify" access permission for the deal

The method `crm.deal.productrows.set` sets (creates or updates) the product rows of a deal.

#|
|| **Name**
`type` | **Description** ||
|| **id^*^**
[`integer`](../../data-types.md) | Identifier of the deal. Can be obtained using the method to get the list of deals: [`crm.deal.list`](./crm-deal-list.md) or when creating a deal: [`crm.deal.add`](./crm-deal-add.md) ||
|| **rows**
[`object[]`](#productrows) | Product rows

An array of objects in the following format: 
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where
- `field_n` — name of the product row field
- `value_n` — value of this field

The list of available fields is described [below](#parameter-rows). ||
|#

### List of Available Fields for Product Rows {#parameter-rows}

#|
|| **Name**
`type` | **Description** ||
|| **PRODUCT_ID**
[`integer`](../../data-types.md) | Identifier of the product in the catalog. Can get the list of products using the method [`catalog.product.list`](../../catalog/product/catalog-product-list.md)
`0` if not from the catalog

Default - `0`
||
|| **PRODUCT_NAME**
[`string`](../../data-types.md) | Name of the product row. If `PRODUCT_ID` is provided, the name will be taken from the product

If both `PRODUCT_ID` and `PRODUCT_NAME` are not provided, then `PRODUCT_NAME` will be equal to `[{id}]`, where `{id}` is the identifier of the created product row
||
|| **PRICE**
[`double`](../../data-types.md) | Final cost of the product per unit

Default - `0.0`
||
|| **PRICE_EXCLUSIVE**
[`double`](../../data-types.md) | Cost per unit considering discounts, excluding taxes ||
|| **PRICE_NETTO**
[`double`](../../data-types.md) | Cost per unit excluding discounts and taxes ||
|| **PRICE_BRUTTO**
[`double`](../../data-types.md) | Cost per unit excluding discounts, but including taxes ||
|| **QUANTITY**
[`double`](../../data-types.md) | Quantity of product units

Default - `1.0`
||
|| **DISCOUNT_TYPE_ID**
[`integer`](../../data-types.md) | Type of discount
Possible types:
- `1` - Absolute
- `2` - Percentage

Default - `2`
||
|| **DISCOUNT_RATE**
[`double`](../../data-types.md) | Discount value in percentage (if using the percentage discount type)

Default - `0.0`
||
|| **DISCOUNT_SUM**
[`double`](../../data-types.md) | Absolute value of the discount (if using the absolute discount type)

Default - `0.0`
||
|| **TAX_RATE**
[`double`](../../data-types.md) | Tax rate in percentage ||
|| **TAX_INCLUDED**
[`boolean`](../../data-types.md) | Indicator of whether the tax is included in the price
Possible values:
- `Y` – tax included
- `N` – tax not included

Default - `N`
||
|| **MEASURE_CODE**
[`catalog_measure.code`](../../catalog/data-types.md#catalog_measure) | Unit of measure code

If `PRODUCT_ID` is provided, the default value will be taken from the product, otherwise `0`
||
|| **MEASURE_NAME**
[`string`](../../data-types.md) | Text representation of the unit of measure (e.g., pcs, kg, m, l, etc.)

If `PRODUCT_ID` is provided, the default value will be taken from the product, otherwise `""`
||
|| **SORT**
[`integer`](../../data-types.md) | Sorting 

Default - `0`
||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5,"rows":[{"PRODUCT_ID":456,"PRICE":1000,"QUANTITY":10,"DISCOUNT_TYPE_ID":1,"DISCOUNT_SUM":100,"TAX_RATE":13,"MEASURE_CODE":796,"MEASURE_NAME":"pcs","SORT":10},{"PRODUCT_NAME":"Product #2","PRICE":500,"QUANTITY":5,"DISCOUNT_TYPE_ID":2,"DISCOUNT_RATE":10,"TAX_RATE":10,"MEASURE_CODE":166,"MEASURE_NAME":"kg","SORT":20}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.productrows.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5,"rows":[{"PRODUCT_ID":456,"PRICE":1000,"QUANTITY":10,"DISCOUNT_TYPE_ID":1,"DISCOUNT_SUM":100,"TAX_RATE":13,"MEASURE_CODE":796,"MEASURE_NAME":"pcs","SORT":10},{"PRODUCT_NAME":"Product #2","PRICE":500,"QUANTITY":5,"DISCOUNT_TYPE_ID":2,"DISCOUNT_RATE":10,"TAX_RATE":10,"MEASURE_CODE":166,"MEASURE_NAME":"kg","SORT":20}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.productrows.set
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.deal.productrows.set',
        {
            id: 5,
            rows: [
                {
                    PRODUCT_ID: 456,
                    PRICE: 1000,
                    QUANTITY: 10,
                    DISCOUNT_TYPE_ID: 1,
                    DISCOUNT_SUM: 100,
                    TAX_RATE: 13,
                    MEASURE_CODE: 796,
                    MEASURE_NAME: "pcs",
                    SORT: 10,
                },
                {
                    PRODUCT_NAME: "Product #2",
                    PRICE: 500,
                    QUANTITY: 5,
                    DISCOUNT_TYPE_ID: 2,
                    DISCOUNT_RATE: 10,
                    TAX_RATE: 10,
                    MEASURE_CODE: 166,
                    MEASURE_NAME: "kg",
                    SORT: 20,
                },
            ],
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.productrows.set',
        [
            'id' => 5,
            'rows' => [
                [
                    'PRODUCT_ID' => 456,
                    'PRICE' => 1000,
                    'QUANTITY' => 10,
                    'DISCOUNT_TYPE_ID' => 1,
                    'DISCOUNT_SUM' => 100,
                    'TAX_RATE' => 13,
                    'MEASURE_CODE' => 796,
                    'MEASURE_NAME' => "pcs",
                    'SORT' => 10,
                ],
                [
                    'PRODUCT_NAME' => "Product #2",
                    'PRICE' => 500,
                    'QUANTITY' => 5,
                    'DISCOUNT_TYPE_ID' => 2,
                    'DISCOUNT_RATE' => 10,
                    'TAX_RATE' => 10,
                    'MEASURE_CODE' => 166,
                    'MEASURE_NAME' => "kg",
                    'SORT' => 20,
                ],
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
  "result": true,
  "time": {
    "start": 1735134795.171273,
    "finish": 1735134796.404978,
    "duration": 1.2337050437927246,
    "processing": 0.9462978839874268,
    "date_start": "2024-12-25T15:53:15+02:00",
    "date_finish": "2024-12-25T15:53:16+02:00",
    "operating": 0
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
  "error": "ERROR_CORE",
  "error_description": "Discount Sum (DISCOUNT_SUM) is required if Percentage Discount Type (DISCOUNT_TYPE_ID) is defined and Discount Rate (DISCOUNT_RATE) is 100%<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Description** | **Value** ||
|| The parameter id is invalid or not defined. | The value provided for the `id` parameter is incorrect ||
|| Access denied | The user does not have permission to "modify" the deal  ||
|| Not found | The deal with the provided `id` was not found ||
|| Discount Rate (`DISCOUNT_RATE`) is required if Percentage Discount Type (`DISCOUNT_TYPE_ID`) is defined. | `DISCOUNT_TYPE_ID = 2` was provided without `DISCOUNT_RATE` ||
|| Discount Sum (`DISCOUNT_SUM`) is required if Percentage Discount Type (`DISCOUNT_TYPE_ID`) is defined and Discount Rate (`DISCOUNT_RATE`) is 100% | `DISCOUNT_RATE = 100` and `DISCOUNT_TYPE_ID = 2` were provided without `DISCOUNT_SUM` ||
|| Discount Sum (`DISCOUNT_SUM`) is required if Monetary Discount Type (`DISCOUNT_TYPE_ID`) is defined. | `DISCOUNT_TYPE_ID = 1` was provided without `DISCOUNT_SUM` ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-productrows-get.md)