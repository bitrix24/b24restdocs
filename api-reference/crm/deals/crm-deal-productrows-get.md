# Get Deal Product Rows crm.deal.productrows.get

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with "read" access permission for the deal

The method `crm.deal.productrows.get` returns the product rows of a deal.

#|
|| **Name**
`type` | **Description** ||
|| **id^*^**
[`integer`](../../data-types.md) | Identifier of the deal. Can be obtained using the method to get the list of deals: [`crm.deal.list`](./crm-deal-list.md) or when creating a deal: [`crm.deal.add`](./crm-deal-add.md) ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}


## Code Examples

Get the product rows of the deal with `id = 5`

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
	-H "Content-Type: application/json" \
	-H "Accept: application/json" \
	-d '{"id":5}' \
	https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.productrows.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
	-H "Content-Type: application/json" \
	-H "Accept: application/json" \
	-d '{"id":5,"auth":"**put_access_token_here**"}' \
	https://**put_your_bitrix24_address**/rest/crm.deal.productrows.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.productrows.get',
    		{
    			id: 5,
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
                'crm.deal.productrows.get',
                [
                    'id' => 5,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting deal product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.productrows.get',
        {
            id: 5,
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
		'crm.deal.productrows.get',
		[
			'id' => 5
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
			"ID": "5",
			"OWNER_ID": "5",
			"OWNER_TYPE": "D",
			"PRODUCT_ID": 450,
			"PRODUCT_NAME": "Product #2",
			"ORIGINAL_PRODUCT_NAME": "Product #2",
			"PRODUCT_DESCRIPTION": null,
			"PRICE": 899.1,
			"PRICE_EXCLUSIVE": 899.1,
			"PRICE_NETTO": 999,
			"PRICE_BRUTTO": 999,
			"PRICE_ACCOUNT": "899.10",
			"QUANTITY": 1,
			"DISCOUNT_TYPE_ID": 2,
			"DISCOUNT_RATE": 10,
			"DISCOUNT_SUM": 99.9,
			"TAX_RATE": null,
			"TAX_INCLUDED": "Y",
			"CUSTOMIZED": "Y",
			"MEASURE_CODE": 796,
			"MEASURE_NAME": "pcs",
			"SORT": 10,
			"XML_ID": "sale_basket_651",
			"TYPE": 1,
			"STORE_ID": 0,
			"RESERVE_ID": 31,
			"DATE_RESERVE_END": "12/26/2024",
			"RESERVE_QUANTITY": 1
		},
		{
			"ID": "4",
			"OWNER_ID": "5",
			"OWNER_TYPE": "D",
			"PRODUCT_ID": 449,
			"PRODUCT_NAME": "Product #1",
			"ORIGINAL_PRODUCT_NAME": "Product #1",
			"PRODUCT_DESCRIPTION": "Detailed description",
			"PRICE": 100,
			"PRICE_EXCLUSIVE": 100,
			"PRICE_NETTO": 100,
			"PRICE_BRUTTO": 100,
			"PRICE_ACCOUNT": "100.00",
			"QUANTITY": 1,
			"DISCOUNT_TYPE_ID": 2,
			"DISCOUNT_RATE": 0,
			"DISCOUNT_SUM": 0,
			"TAX_RATE": null,
			"TAX_INCLUDED": "Y",
			"CUSTOMIZED": "Y",
			"MEASURE_CODE": 796,
			"MEASURE_NAME": "pcs",
			"SORT": 20,
			"XML_ID": "sale_basket_650",
			"TYPE": 1,
			"STORE_ID": 1,
			"RESERVE_ID": 30,
			"DATE_RESERVE_END": "12/26/2024",
			"RESERVE_QUANTITY": 1
		}
	],
	"time": {
		"start": 1734969122.936213,
		"finish": 1734969123.586089,
		"duration": 0.6498758792877197,
		"processing": 0.14046597480773926,
		"date_start": "2024-12-23T17:52:02+02:00",
		"date_finish": "2024-12-23T17:52:03+02:00",
		"operating": 0
	}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`productrow[]`](#productrow) | Root element of the response containing an array of deal product rows ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Product Row Type {#productrow}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the product row ||
|| **OWNER_ID**
[`integer`](../../data-types.md) | Identifier of the entity to which the product is linked. For this method, it will always equal the `id` of the deal ||
|| **OWNER_TYPE**
[`string`](../../data-types.md) | String identifier of the entity type to which the product is linked. For this method, it will always equal `D` ||
|| **PRODUCT_ID**
[`integer`](../../data-types.md) | Identifier of the product in the catalog. `0` if not from the catalog

For more detailed information about the product, use [`catalog.product.get`](../../catalog/product/catalog-product-get.md)
||
|| **PRODUCT_NAME**
[`string`](../../data-types.md) | Name of the product row ||
|| **ORIGINAL_PRODUCT_NAME**
[`string`](../../data-types.md) | Name of the product row in the catalog ||
|| **PRODUCT_DESCRIPTION**
[`string`](../../data-types.md) | Description of the product row ||
|| **PRICE**
[`double`](../../data-types.md) | Final cost of the product per unit ||
|| **PRICE_EXCLUSIVE**
[`double`](../../data-types.md) | Cost per unit considering discounts, excluding taxes ||
|| **PRICE_NETTO**
[`double`](../../data-types.md) | Cost per unit excluding discounts and taxes ||
|| **PRICE_BRUTTO**
[`double`](../../data-types.md) | Cost per unit excluding discounts but including taxes ||
|| **PRICE_ACCOUNT**
[`string`](../../data-types.md) | Cost of the product in "report currency" ||
|| **QUANTITY**
[`integer`](../../data-types.md) | Quantity of product units ||
|| **DISCOUNT_TYPE_ID**
[`integer`](../../data-types.md) | Type of discount
Possible types:
 - `1` - Absolute
 - `2` - Percentage

||
|| **DISCOUNT_RATE**
[`double`](../../data-types.md) | Discount value in percentage (if using the percentage discount type) ||
|| **DISCOUNT_SUM**
[`double`](../../data-types.md) | Absolute discount value (if using the absolute discount type) ||
|| **TAX_RATE**
[`double`](../../data-types.md) | Tax rate in percentage ||
|| **TAX_INCLUDED**
[`char`](../../data-types.md) | Indicator of whether tax is included in the price
Possible values:
- `Y` – tax included
- `N` – tax not included

||
|| **CUSTOMIZED**
[`char`](../../data-types.md) | Customized (Deprecated)
Possible values:
 - `Y` - Yes
 - `N` - No

||
|| **MEASURE_CODE**
[`catalog_measure.code`](../../catalog/data-types.md#catalog_measure) | Measure unit code ||
|| **MEASURE_NAME**
[`string`](../../data-types.md) | Text representation of the measure unit (e.g., pcs, kg, m, l, etc.) ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting ||
|| **XML_ID**
[`string`](../../data-types.md) | External code of the product ||
|| **TYPE**
[`integer`](../../data-types.md) | Type of product
Possible values: 
 - `1` - Simple product
 - `4` - Trade offer/variation
 - `7` - Service

||
|| **STORE_ID**
[`integer`](../../data-types.md) | Identifier of the inventory. For detailed information about the inventory, use [`catalog.store.get`](../../catalog/store/catalog-store-get.md) ||
|| **RESERVE_ID**
[`integer`](../../data-types.md) | Identifier of the reserve ||
|| **DATE_RESERVE_END**
[`date`](../../data-types.md) | Date of reserve expiration ||
|| **RESERVE_QUANTITY**
[`integer`](../../data-types.md) | Quantity of reserved product units ||
|#


## Error Handling

HTTP Status: **400**

```json
{
  "error": "",
  "error_description": "The parameter id is invalid or not defined."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Description** | **Value** ||
|| The parameter id is invalid or not defined. | The parameter `id` has an incorrect value ||
|| Access denied | The user does not have permission to "read" the deal  ||
|| Not found | The deal with the provided `id` was not found ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-productrows-set.md)