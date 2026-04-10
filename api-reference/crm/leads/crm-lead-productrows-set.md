# Set Products in Lead crm.lead.productrows.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for the lead

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.productrow.*](../universal/product-rows/index.md).

{% endnote %}

The method `crm.lead.productrows.set` creates or updates the product rows of a lead. Existing rows that are not passed to the method will be removed from the lead.

To modify only a single row, use the methods [crm.item.productrow.*](../universal/product-rows/index.md).

## Method Parameters

{% include [Parameter Note](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id^*^**
[`integer`](../../data-types.md) | Lead identifier. Can be obtained using the method to retrieve the list of leads: [`crm.lead.list`](./crm-lead-list.md) or when creating a lead: [`crm.lead.add`](./crm-lead-add.md) ||
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

The list of available fields is described [below](#parameter-rows) ||
|#

### List of Available Fields for Product Rows {#parameter-rows}

#|
|| **Name**
`type` | **Description** ||
|| **PRODUCT_ID**
[`integer`](../../data-types.md) | Identifier of the product in the catalog. You can get the list of products using the method [`catalog.product.list`](../../catalog/product/catalog-product-list.md)
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
[`double`](../../data-types.md) | Cost per unit excluding discounts but including taxes ||
|| **QUANTITY**
[`double`](../../data-types.md) | Quantity of the product

Default - `1.0`
||
|| **DISCOUNT_TYPE_ID**
[`integer`](../../data-types.md) | Discount type
Possible types:
- `1` - Absolute
- `2` - Percentage

Default - `2`
||
|| **DISCOUNT_RATE**
[`double`](../../data-types.md) | Discount value in percentage (if percentage discount type is used)

Default - `0.0`
||
|| **DISCOUNT_SUM**
[`double`](../../data-types.md) | Absolute discount value (if absolute discount type is used)

Default - `0.0`
||
|| **TAX_RATE**
[`double`](../../data-types.md) | Tax rate in percentage ||
|| **TAX_INCLUDED**
[`char`](../../data-types.md) | Indicator of whether tax is included in the price
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

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5,"rows":[{"PRODUCT_ID":456,"PRICE":1000,"QUANTITY":10,"DISCOUNT_TYPE_ID":1,"DISCOUNT_SUM":100,"TAX_RATE":13,"MEASURE_CODE":796,"MEASURE_NAME":"pcs","SORT":10},{"PRODUCT_NAME":"Product #2","PRICE":500,"QUANTITY":5,"DISCOUNT_TYPE_ID":2,"DISCOUNT_RATE":10,"TAX_RATE":10,"MEASURE_CODE":166,"MEASURE_NAME":"kg","SORT":20}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.productrows.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5,"rows":[{"PRODUCT_ID":456,"PRICE":1000,"QUANTITY":10,"DISCOUNT_TYPE_ID":1,"DISCOUNT_SUM":100,"TAX_RATE":13,"MEASURE_CODE":796,"MEASURE_NAME":"pcs","SORT":10},{"PRODUCT_NAME":"Product #2","PRICE":500,"QUANTITY":5,"DISCOUNT_TYPE_ID":2,"DISCOUNT_RATE":10,"TAX_RATE":10,"MEASURE_CODE":166,"MEASURE_NAME":"kg","SORT":20}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.productrows.set
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'crm.lead.productrows.set',
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
            }
        );

        const result = response.getData().result;
        result.error()
            ? console.error(result.error())
            : console.info(result)
        ;
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.lead.productrows.set',
                [
                    'id'   => 5,
                    'rows' => [
                        [
                            'PRODUCT_ID'       => 456,
                            'PRICE'           => 1000,
                            'QUANTITY'        => 10,
                            'DISCOUNT_TYPE_ID' => 1,
                            'DISCOUNT_SUM'    => 100,
                            'TAX_RATE'        => 13,
                            'MEASURE_CODE'    => 796,
                            'MEASURE_NAME'    => "pcs",
                            'SORT'            => 10,
                        ],
                        [
                            'PRODUCT_NAME'     => "Product #2",
                            'PRICE'           => 500,
                            'QUANTITY'        => 5,
                            'DISCOUNT_TYPE_ID' => 2,
                            'DISCOUNT_RATE'   => 10,
                            'TAX_RATE'        => 10,
                            'MEASURE_CODE'    => 166,
                            'MEASURE_NAME'    => "kg",
                            'SORT'            => 20,
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting lead product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.lead.productrows.set',
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.lead.productrows.set',
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
    "start": 1772549200,
    "finish": 1772549200.281883,
    "duration": 0.28188300132751465,
    "processing": 0,
    "date_start": "2026-03-03T17:46:40+01:00",
    "date_finish": "2026-03-03T17:46:40+01:00",
    "operating_reset_at": 1772549800,
    "operating": 0.9811058044433594
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
|| The parameter id is invalid or not defined | An incorrect value was passed to the `id` parameter ||
|| The parameter rows must be array | A non-array value was passed to the `rows` parameter ||
|| Access denied | The user does not have permission to "edit" the lead ||
|| Not found | The lead with the provided `id` was not found ||
|| Discount Rate (`DISCOUNT_RATE`) is required if Percentage Discount Type (`DISCOUNT_TYPE_ID`) is defined | `DISCOUNT_TYPE_ID = 2` was passed without `DISCOUNT_RATE` ||
|| Discount Sum (`DISCOUNT_SUM`) is required if Percentage Discount Type (`DISCOUNT_TYPE_ID`) is defined and Discount Rate (`DISCOUNT_RATE`) is 100% | `DISCOUNT_RATE = 100` and `DISCOUNT_TYPE_ID = 2` were passed without `DISCOUNT_SUM` ||
|| Discount Sum (`DISCOUNT_SUM`) is required if Monetary Discount Type (`DISCOUNT_TYPE_ID`) is defined | `DISCOUNT_TYPE_ID = 1` was passed without `DISCOUNT_SUM` ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-lead-productrows-get.md)