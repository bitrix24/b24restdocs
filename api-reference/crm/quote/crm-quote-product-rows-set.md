# Set Product Rows for the Quote `crm.quote.productrows.set`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for estimates

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.productrow.*](../universal/product-rows/index.md).

{% endnote %}

The method `crm.quote.productrows.set` creates or updates product rows in an estimate.

To modify only a single row, use the methods [crm.item.productrow.*](../universal/product-rows/index.md).

## Method Parameters

{% include [Parameter Note](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Identifier of the estimate.

The identifier can be obtained using the methods [crm.quote.list](./crm-quote-list.md) or [crm.quote.add](./crm-quote-add.md) ||
|| **rows**
[`object[]`](#parameter-rows) | Array of product rows.

Format of an array element:
```json
{
    "field_1": "value_1",
    "field_2": "value_2",
    "...": "..."
}
```

where:
- `field_n` — name of the product row field
- `value_n` — value of the field

A list of primary fields is described [below](#parameter-rows) ||
|#

### List of Available Fields for Product Rows {#parameter-rows}

#|
|| **Name**
`type` | **Description** ||
|| **PRODUCT_ID**
[`integer`](../data-types.md) | Identifier of the product in the catalog.

The list of products can be obtained using the method [catalog.product.list](../../catalog/product/catalog-product-list.md).

If `PRODUCT_ID = 0`, the row is created as "custom" ||
|| **PRODUCT_NAME**
[`string`](../data-types.md) | Name of the product row ||
|| **PRODUCT_DESCRIPTION**
[`string`](../data-types.md) | Description of the product row ||
|| **PRICE**
[`double`](../data-types.md) | Final cost of the product per unit ||
|| **QUANTITY**
[`double`](../data-types.md) | Number of product units ||
|| **DISCOUNT_TYPE_ID**
[`integer`](../data-types.md) | Type of discount:
- `1` — absolute
- `2` — percentage ||
|| **DISCOUNT_RATE**
[`double`](../data-types.md) | Discount value in percentage ||
|| **DISCOUNT_SUM**
[`double`](../data-types.md) | Absolute discount value ||
|| **TAX_RATE**
[`double`](../data-types.md) | Tax rate in percentage ||
|| **TAX_INCLUDED**
[`char`](../data-types.md) | Is tax included in the price:
- `Y` — yes
- `N` — no ||
|| **MEASURE_CODE**
[`catalog_measure.code`](../../catalog/data-types.md#catalog_measure) | Unit of measure code ||
|| **MEASURE_NAME**
[`string`](../data-types.md) | Text representation of the unit of measure ||
|| **SORT**
[`integer`](../data-types.md) | Sorting ||
|#

The complete list of fields for product rows and types can be obtained using the method [crm.productrow.fields](../outdated/productrow-old/crm-productrow-fields.md).

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

Set two product rows for the estimate with `id = 1`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"rows":[{"PRODUCT_ID":459,"PRICE":3000,"QUANTITY":1,"DISCOUNT_TYPE_ID":2,"DISCOUNT_RATE":0,"TAX_RATE":0,"TAX_INCLUDED":"Y","MEASURE_CODE":796,"MEASURE_NAME":"pcs","SORT":10},{"PRODUCT_NAME":"Support Service","PRICE":1500,"QUANTITY":2,"DISCOUNT_TYPE_ID":2,"DISCOUNT_RATE":0,"TAX_RATE":0,"TAX_INCLUDED":"Y","MEASURE_CODE":796,"MEASURE_NAME":"pcs","SORT":20}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.productrows.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"rows":[{"PRODUCT_ID":459,"PRICE":3000,"QUANTITY":1,"DISCOUNT_TYPE_ID":2,"DISCOUNT_RATE":0,"TAX_RATE":0,"TAX_INCLUDED":"Y","MEASURE_CODE":796,"MEASURE_NAME":"pcs","SORT":10},{"PRODUCT_NAME":"Support Service","PRICE":1500,"QUANTITY":2,"DISCOUNT_TYPE_ID":2,"DISCOUNT_RATE":0,"TAX_RATE":0,"TAX_INCLUDED":"Y","MEASURE_CODE":796,"MEASURE_NAME":"pcs","SORT":20}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.productrows.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.quote.productrows.set',
    		{
    			id: 1,
    			rows: [
    				{
    					PRODUCT_ID: 459,
    					PRICE: 3000,
    					QUANTITY: 1,
    					DISCOUNT_TYPE_ID: 2,
    					DISCOUNT_RATE: 0,
    					TAX_RATE: 0,
    					TAX_INCLUDED: 'Y',
    					MEASURE_CODE: 796,
    					MEASURE_NAME: 'pcs',
    					SORT: 10,
    				},
    				{
    					PRODUCT_NAME: 'Support Service',
    					PRICE: 1500,
    					QUANTITY: 2,
    					DISCOUNT_TYPE_ID: 2,
    					DISCOUNT_RATE: 0,
    					TAX_RATE: 0,
    					TAX_INCLUDED: 'Y',
    					MEASURE_CODE: 796,
    					MEASURE_NAME: 'pcs',
    					SORT: 20,
    				},
    			],
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
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
                'crm.quote.productrows.set',
                [
                    'id' => 1,
                    'rows' => [
                        [
                            'PRODUCT_ID' => 459,
                            'PRICE' => 3000,
                            'QUANTITY' => 1,
                            'DISCOUNT_TYPE_ID' => 2,
                            'DISCOUNT_RATE' => 0,
                            'TAX_RATE' => 0,
                            'TAX_INCLUDED' => 'Y',
                            'MEASURE_CODE' => 796,
                            'MEASURE_NAME' => 'pcs',
                            'SORT' => 10,
                        ],
                        [
                            'PRODUCT_NAME' => 'Support Service',
                            'PRICE' => 1500,
                            'QUANTITY' => 2,
                            'DISCOUNT_TYPE_ID' => 2,
                            'DISCOUNT_RATE' => 0,
                            'TAX_RATE' => 0,
                            'TAX_INCLUDED' => 'Y',
                            'MEASURE_CODE' => 796,
                            'MEASURE_NAME' => 'pcs',
                            'SORT' => 20,
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Updated: ' . ($result ? 'true' : 'false');

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting quote product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.productrows.set',
        {
            id: 1,
            rows: [
                {
                    PRODUCT_ID: 459,
                    PRICE: 3000,
                    QUANTITY: 1,
                    DISCOUNT_TYPE_ID: 2,
                    DISCOUNT_RATE: 0,
                    TAX_RATE: 0,
                    TAX_INCLUDED: 'Y',
                    MEASURE_CODE: 796,
                    MEASURE_NAME: 'pcs',
                    SORT: 10,
                },
                {
                    PRODUCT_NAME: 'Support Service',
                    PRICE: 1500,
                    QUANTITY: 2,
                    DISCOUNT_TYPE_ID: 2,
                    DISCOUNT_RATE: 0,
                    TAX_RATE: 0,
                    TAX_INCLUDED: 'Y',
                    MEASURE_CODE: 796,
                    MEASURE_NAME: 'pcs',
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
        'crm.quote.productrows.set',
        [
            'id' => 1,
            'rows' => [
                [
                    'PRODUCT_ID' => 459,
                    'PRICE' => 3000,
                    'QUANTITY' => 1,
                    'DISCOUNT_TYPE_ID' => 2,
                    'DISCOUNT_RATE' => 0,
                    'TAX_RATE' => 0,
                    'TAX_INCLUDED' => 'Y',
                    'MEASURE_CODE' => 796,
                    'MEASURE_NAME' => 'pcs',
                    'SORT' => 10,
                ],
                [
                    'PRODUCT_NAME' => 'Support Service',
                    'PRICE' => 1500,
                    'QUANTITY' => 2,
                    'DISCOUNT_TYPE_ID' => 2,
                    'DISCOUNT_RATE' => 0,
                    'TAX_RATE' => 0,
                    'TAX_INCLUDED' => 'Y',
                    'MEASURE_CODE' => 796,
                    'MEASURE_NAME' => 'pcs',
                    'SORT' => 20,
                ],
            ],
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
        "start": 1773416018,
        "finish": 1773416018.877651,
        "duration": 0.8776509761810303,
        "processing": 0,
        "date_start": "2026-03-13T18:33:38+01:00",
        "date_finish": "2026-03-13T18:33:38+01:00",
        "operating_reset_at": 1773416618,
        "operating": 0.5666530132293701
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Root element of the response. Contains:
- `true` — product rows successfully saved
- `false` — product rows not saved ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Not found."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `The parameter id is invalid or not defined.` | The parameter `id` has no value or an invalid value was provided ||
|| `-` | `The parameter rows must be array.` | The parameter `rows` is not an array ||
|| `-` | `Access denied.` | The user does not have permission to edit the estimate ||
|| `-` | `Not found.` | The estimate with the provided `id` was not found ||
|| `-` | Catalog rights check error message | Error checking rights for catalog products and/or catalog restrictions for the provided rows ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-product-rows-get.md)
- [{#T}](./crm-quote-get.md)
- [{#T}](./crm-quote-update.md)
- [{#T}](./crm-quote-add.md)
- [{#T}](./crm-quote-fields.md)