# Update Product Row of CRM Object crm.item.productrow.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires permission to modify the CRM object whose product row is being updated.

This method updates the product row of a CRM object.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`crm_item_product_row.id`](../../data-types.md#crm_item_product_row) | Identifier of the product row ||
|| **fields***
[`object`](../../../data-types.md) | Object containing field values to update the product row of the CRM object ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **productId**
[`catalog_product.id`](../../../catalog/data-types.md#catalog_product) | Identifier of the product from the catalog ||
|| **productName**
[`string`](../../../data-types.md) | Name of the product in the product row ||
|| **price**
[`double`](../../../data-types.md) | Price per unit of the product row including discounts and taxes ||
|| **quantity**
[`double`](../../../data-types.md) | Quantity of the product ||
|| **discountTypeId**
[`integer`](../../../data-types.md) | Type of discount.
Possible values:
- `1` — absolute value
- `2` — percentage value ||
|| **discountRate**
[`double`](../../../data-types.md) | Discount value in percentage (if using percentage discount type) ||
|| **discountSum**
[`double`](../../../data-types.md) | Absolute discount value (if using absolute discount type) ||
|| **taxRate**
[`double`](../../../data-types.md) | Tax rate in percentage ||
|| **taxIncluded**
[`string`](../../../data-types.md) | Indicator of whether tax is included in the price.
Possible values:
- `Y` – tax included
- `N` – tax not included  ||
|| **measureCode**
[`catalog_measure.code`](../../../catalog/data-types.md#catalog_measure) | Unit of measure code ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17648,"fields":{"productId":9621,"price":90000,"quantity":3,"discountTypeId":2,"discountRate":10,"taxRate":10,"taxIncluded":"Y","measureCode":796,"sort":20}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.productrow.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17648,"fields":{"productId":9621,"price":90000,"quantity":3,"discountTypeId":2,"discountRate":10,"taxRate":10,"taxIncluded":"Y","measureCode":796,"sort":20},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.productrow.update
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.item.productrow.update', {
            id: 17648,
            fields: {
                productId: 9621,
                price: 90000.000000,
                quantity: 3,
                discountTypeId: 2,
                discountRate: 10,
                taxRate: 10,
                taxIncluded: 'Y',
                measureCode: 796,
                sort: 20,
            },
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.productrow.update',
        [
            'id' => 17648,
            'fields' => [
                'productId' => 9621,
                'price' => 90000.000000,
                'quantity' => 3,
                'discountTypeId' => 2,
                'discountRate' => 10,
                'taxRate' => 10,
                'taxIncluded' => 'Y',
                'measureCode' => 796,
                'sort' => 20,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP Status: **200**

```json
{
   "result":{
      "productRow":{
         "id":17649,
         "ownerId":13142,
         "ownerType":"D",
         "productId":9621,
         "productName":"iphone 14",
         "price":90000,
         "priceAccount":90000,
         "priceExclusive":81818.18,
         "priceNetto":90909.09,
         "priceBrutto":100000,
         "quantity":3,
         "discountTypeId":2,
         "discountRate":10,
         "discountSum":9090.91,
         "taxRate":10,
         "taxIncluded":"Y",
         "customized":"Y",
         "measureCode":796,
         "measureName":"pcs",
         "sort":20,
         "xmlId":"sale_basket_8147",
         "type":4
      }
   },
   "time":{
      "start":1716890008.714214,
      "finish":1716890010.275307,
      "duration":1.5610928535461426,
      "processing":1.3967258930206299,
      "date_start":"2024-05-28T12:53:28+03:00",
      "date_finish":"2024-05-28T12:53:30+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **productRow**
[`crm_item_product_row`](../../data-types.md#crm_item_product_row) | Object containing information about the updated product row ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error":"NOT_FOUND",
   "error_description":"Element not found"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | Working with this type of objects is not supported ||
|| `ACCESS_DENIED` | Access denied ||
|| `NOT_FOUND` | Product row not found ||
|| `INVALID_ARG_VALUE` | Unknown field or the provided field is not available for update ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-productrow-add.md)
- [{#T}](./crm-item-productrow-fields.md)
- [{#T}](./crm-item-productrow-get.md)
- [{#T}](./crm-item-productrow-set.md)
- [{#T}](./crm-item-productrow-get-available-for-payment.md)
- [{#T}](./crm-item-productrow-list.md)
- [{#T}](./crm-item-productrow-delete.md)