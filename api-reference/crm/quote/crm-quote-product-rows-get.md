# Retrieve Product Rows of the Estimate crm.quote.productrows.get

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "read" access permission for estimates

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.productrow.*](../universal/product-rows/index.md).

{% endnote %}

The method `crm.quote.productrows.get` returns the product rows of an estimate.

## Method Parameters

{% include [Note on Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Identifier of the estimate.

The identifier can be obtained using the methods [crm.quote.list](./crm-quote-list.md) or [crm.quote.add](./crm-quote-add.md) ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

Retrieve product rows of the estimate with `id = 1`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.productrows.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.productrows.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.quote.productrows.get',
    		{
    			id: 1,
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
                'crm.quote.productrows.get',
                [
                    'id' => 1,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting quote product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.productrows.get',
        {
            id: 1,
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
        'crm.quote.productrows.get',
        [
            'id' => 1,
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
    "result": [
        {
            "ID": "77",
            "OWNER_ID": "1",
            "OWNER_TYPE": "Q",
            "PRODUCT_ID": 459,
            "PRODUCT_NAME": "Two-Week Subscription",
            "ORIGINAL_PRODUCT_NAME": "Two-Week Subscription",
            "PRODUCT_DESCRIPTION": "",
            "PRICE": 3000,
            "PRICE_EXCLUSIVE": 3000,
            "PRICE_NETTO": 0,
            "PRICE_BRUTTO": 0,
            "PRICE_ACCOUNT": "3000.00000000",
            "QUANTITY": 1,
            "DISCOUNT_TYPE_ID": 2,
            "DISCOUNT_RATE": 0,
            "DISCOUNT_SUM": 0,
            "TAX_RATE": 0,
            "TAX_INCLUDED": "Y",
            "CUSTOMIZED": "Y",
            "MEASURE_CODE": 796,
            "MEASURE_NAME": "pcs",
            "SORT": 10,
            "XML_ID": null,
            "TYPE": 1,
            "STORE_ID": null,
            "RESERVE_ID": null,
            "DATE_RESERVE_END": null,
            "RESERVE_QUANTITY": null
        }
    ],
    "time": {
        "start": 1773415393,
        "finish": 1773415393.257197,
        "duration": 0.25719690322875977,
        "processing": 0,
        "date_start": "2026-03-13T18:23:13+01:00",
        "date_finish": "2026-03-13T18:23:13+01:00",
        "operating_reset_at": 1773415993,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`productrow[]`](#productrow) | Root element of the response containing an array of product rows of the estimate ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

#### Type productrow {#productrow}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the product row ||
|| **OWNER_ID**
[`integer`](../data-types.md) | Identifier of the element to which the product is linked. For this method, it equals the `id` of the estimate ||
|| **OWNER_TYPE**
[`string`](../data-types.md) | String identifier of the CRM object type to which the product is linked. For this method, it is always `Q` ||
|| **PRODUCT_ID**
[`integer`](../data-types.md) | Identifier of the product in the catalog. `0` if the product is not from the catalog.

To get detailed information about the product, use [catalog.product.get](../../catalog/product/catalog-product-get.md) ||
|| **PRODUCT_NAME**
[`string`](../data-types.md) | Name of the product row ||
|| **ORIGINAL_PRODUCT_NAME**
[`string`](../data-types.md) | Name of the product row in the catalog ||
|| **PRODUCT_DESCRIPTION**
[`string`](../data-types.md) | Description of the product row ||
|| **PRICE**
[`double`](../data-types.md) | Final cost of the product per unit ||
|| **PRICE_EXCLUSIVE**
[`double`](../data-types.md) | Cost per unit considering discounts, excluding taxes ||
|| **PRICE_NETTO**
[`double`](../data-types.md) | Cost per unit excluding discounts and taxes ||
|| **PRICE_BRUTTO**
[`double`](../data-types.md) | Cost per unit excluding discounts but including taxes ||
|| **PRICE_ACCOUNT**
[`string`](../data-types.md) | Cost of the product in reporting currency ||
|| **QUANTITY**
[`double`](../data-types.md) | Quantity of product units ||
|| **DISCOUNT_TYPE_ID**
[`integer`](../data-types.md) | Type of discount:
- `1` — absolute
- `2` — percentage ||
|| **DISCOUNT_RATE**
[`double`](../data-types.md) | Discount value in percentage ||
|| **DISCOUNT_SUM**
[`double`](../data-types.md) | Absolute value of the discount ||
|| **TAX_RATE**
[`double`](../data-types.md) | Tax rate in percentage ||
|| **TAX_INCLUDED**
[`char`](../data-types.md) | Whether tax is included in the price:
- `Y` — yes
- `N` — no ||
|| **CUSTOMIZED**
[`char`](../data-types.md) | Indicator of manual modification of the product row:
- `Y` — yes
- `N` — no ||
|| **MEASURE_CODE**
[`catalog_measure.code`](../../catalog/data-types.md#catalog_measure) | Unit of measure code ||
|| **MEASURE_NAME**
[`string`](../data-types.md) | Text representation of the unit of measure ||
|| **SORT**
[`integer`](../data-types.md) | Sorting ||
|| **XML_ID**
[`string`](../data-types.md) | External code of the product row ||
|| **TYPE**
[`integer`](../data-types.md) | Type of product:
- `1` — simple product
- `4` — trade offer (variation)
- `7` — service ||
|| **STORE_ID**
[`integer`](../data-types.md) | Identifier of the warehouse ||
|| **RESERVE_ID**
[`integer`](../data-types.md) | Identifier of the reserve ||
|| **DATE_RESERVE_END**
[`date`](../data-types.md) | Date of reservation end ||
|| **RESERVE_QUANTITY**
[`double`](../data-types.md) | Quantity of reserved product units ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `The parameter id is invalid or not defined.` | The `id` parameter has no value or an invalid value was provided ||
|| `-` | `Access denied.` | The user does not have permission to read the estimate ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-product-rows-set.md)
- [{#T}](./crm-quote-get.md)
- [{#T}](./crm-quote-update.md)
- [{#T}](./crm-quote-add.md)
- [{#T}](./crm-quote-fields.md)