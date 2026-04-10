# Get Lead Product Rows crm.lead.productrows.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with "read" access permission for leads

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [crm.item.productrow.*](../universal/product-rows/index.md).

{% endnote %}

The method `crm.lead.productrows.get` returns the product rows of a lead.

## Method Parameters

{% include [Note on Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id^*^**
[`integer`](../../data-types.md) | Identifier of the lead. Can be obtained using the method to get the list of leads: [`crm.lead.list`](./crm-lead-list.md) or when creating a lead: [`crm.lead.add`](./crm-lead-add.md) ||
|#

## Code Examples

Get product rows for the lead with `id = 5`

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
	-H "Content-Type: application/json" \
	-H "Accept: application/json" \
	-d '{"id":5}' \
	https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.productrows.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
	-H "Content-Type: application/json" \
	-H "Accept: application/json" \
	-d '{"id":5,"auth":"**put_access_token_here**"}' \
	https://**put_your_bitrix24_address**/rest/crm.lead.productrows.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.lead.productrows.get',
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
                'crm.lead.productrows.get',
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
        echo 'Error getting lead product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.lead.productrows.get',
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
		'crm.lead.productrows.get',
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
			"ID": "1425",
			"OWNER_ID": "5",
			"OWNER_TYPE": "L",
			"PRODUCT_ID": 6935,
			"PRODUCT_NAME": "Product Variation #1",
			"ORIGINAL_PRODUCT_NAME": "Product Variation #1",
			"PRODUCT_DESCRIPTION": null,
			"PRICE": 10,
			"PRICE_EXCLUSIVE": 10,
			"PRICE_NETTO": 10,
			"PRICE_BRUTTO": 10,
			"PRICE_ACCOUNT": "10.00000000",
			"QUANTITY": 1,
			"DISCOUNT_TYPE_ID": 2,
			"DISCOUNT_RATE": 0,
			"DISCOUNT_SUM": 0,
			"TAX_RATE": null,
			"TAX_INCLUDED": "N",
			"CUSTOMIZED": "Y",
			"MEASURE_CODE": 796,
			"MEASURE_NAME": "pcs",
			"SORT": 10,
			"XML_ID": "",
			"TYPE": 4,
			"STORE_ID": null,
			"RESERVE_ID": null,
			"DATE_RESERVE_END": null,
			"RESERVE_QUANTITY": null
		},
		{
			"ID": "1429",
			"OWNER_ID": "5",
			"OWNER_TYPE": "L",
			"PRODUCT_ID": 6595,
			"PRODUCT_NAME": "Product #2",
			"ORIGINAL_PRODUCT_NAME": "Product #2",
			"PRODUCT_DESCRIPTION": "Detailed description",
			"PRICE": 22175.328,
			"PRICE_EXCLUSIVE": 19799.4,
			"PRICE_NETTO": 32999,
			"PRICE_BRUTTO": 36958.88,
			"PRICE_ACCOUNT": "22175.32800000",
			"QUANTITY": 1,
			"DISCOUNT_TYPE_ID": 2,
			"DISCOUNT_RATE": 40,
			"DISCOUNT_SUM": 13199.6,
			"TAX_RATE": 12,
			"TAX_INCLUDED": "N",
			"CUSTOMIZED": "Y",
			"MEASURE_CODE": 796,
			"MEASURE_NAME": "pcs",
			"SORT": 30,
			"XML_ID": "",
			"TYPE": 1,
			"STORE_ID": null,
			"RESERVE_ID": null,
			"DATE_RESERVE_END": null,
			"RESERVE_QUANTITY": null
		}
	],
	"time": {
		"start": 1772541576,
		"finish": 1772541577.039689,
		"duration": 1.039689064025879,
		"processing": 0,
		"date_start": "2026-03-03T15:39:36+01:00",
		"date_finish": "2026-03-03T15:39:37+01:00",
		"operating_reset_at": 1772542177,
		"operating": 0
	}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`productrow[]`](#productrow) | Root element of the response containing an array of lead product rows ||
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
[`integer`](../../data-types.md) | Identifier of the element to which the product is linked. For this method, it will always equal the `id` of the lead ||
|| **OWNER_TYPE**
[`string`](../../data-types.md) | String identifier of the type of object to which the product is linked. For this method, it will always equal `L` ||
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
[`double`](../../data-types.md) | Quantity of product units ||
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
[`catalog_measure.code`](../../catalog/data-types.md#catalog_measure) | Unit of measure code ||
|| **MEASURE_NAME**
[`string`](../../data-types.md) | Text representation of the unit of measure (e.g., pcs, kg, m, l, etc.) ||
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
[`integer`](../../data-types.md) | Identifier of the warehouse. For detailed information about the warehouse, use [`catalog.store.get`](../../catalog/store/catalog-store-get.md) ||
|| **RESERVE_ID**
[`integer`](../../data-types.md) | Identifier of the reserve ||
|| **DATE_RESERVE_END**
[`date`](../../data-types.md) | Date of reservation end ||
|| **RESERVE_QUANTITY**
[`double`](../../data-types.md) | Quantity of reserved product units ||
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
|| The parameter id is invalid or not defined | The `id` parameter has an incorrect value ||
|| Access denied | The user does not have permission to "read" the lead ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-lead-productrows-set.md)