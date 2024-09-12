# Add Product Item to CRM Object crm.item.productrow.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires access permission to modify the CRM object to which the product item is being added.

This method adds a product item to a CRM object.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object containing field values for adding the product item to the CRM object ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **ownerId**
[`integer`](../../../data-types.md) | Identifier of the CRM object ||
|| **ownerType**
[`string`](../../../data-types.md) | Identifier of the [`CRM object type`](../../data-types.md#tip-obuekta-crm) ||
|| **productId**
[`catalog_product.id`](../../../catalog/data-types.md#catalog_product) | Identifier of the product from the catalog ||
|| **productName**
[`string`](../../../data-types.md) | Name of the product in the product item.
If not provided, but `productId` is given, the product name from the product catalog is used ||
|| **price**
[`double`](../../../data-types.md) | Price per unit of the product item, including discounts and taxes ||
|| **quantity**
[`double`](../../../data-types.md) | Quantity of the product. 
Defaults to 1 ||
|| **discountTypeId**
[`integer`](../../../data-types.md) | Type of discount.
Possible values:
- `1` — absolute value
- `2` — percentage value
Defaults to 2 ||
|| **discountRate**
[`double`](../../../data-types.md) | Discount value in percentage (if using the percentage discount type) ||
|| **discountSum**
[`double`](../../../data-types.md) | Absolute discount value (if using the absolute discount type) ||
|| **taxRate**
[`double`](../../../data-types.md) | Tax rate in percentage ||
|| **taxIncluded**
[`string`](../../../data-types.md) | Indicator of whether tax is included in the price.
Possible values:
- `Y` – tax included
- `N` – tax not included
Defaults to N ||
|| **measureCode**
[`catalog_measure.code`](../../../catalog/data-types.md#catalog_measure) | Unit of measure code
If not provided and `productId` is given, the unit of measure from the product catalog is used ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ownerId":13142,"ownerType":"D","productId":9621,"price":80000,"quantity":2,"discountTypeId":2,"discountRate":20,"taxRate":20,"taxIncluded":"Y","measureCode":796,"sort":10}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.productrow.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ownerId":13142,"ownerType":"D","productId":9621,"price":80000,"quantity":2,"discountTypeId":2,"discountRate":20,"taxRate":20,"taxIncluded":"Y","measureCode":796,"sort":10},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.productrow.add
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.item.productrow.add', {
            fields: {
                ownerId: 13142,
                ownerType: 'D',
                productId: 9621,
                price: 80000.000000,
                quantity: 2,
                discountTypeId: 2,
                discountRate: 20,
                taxRate: 20,
                taxIncluded: 'Y',
                measureCode: 796,
                sort: 10,
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
        'crm.item.productrow.add',
        [
            'fields' => [
                'ownerId' => 13142,
                'ownerType' => 'D',
                'productId' => 9621,
                'price' => 80000.000000,
                'quantity' => 2,
                'discountTypeId' => 2,
                'discountRate' => 20,
                'taxRate' => 20,
                'taxIncluded' => 'Y',
                'measureCode' => 796,
                'sort' => 10,
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
         "id":17647,
         "ownerId":13142,
         "ownerType":"D",
         "productId":9621,
         "price":80000,
         "quantity":2,
         "discountTypeId":2,
         "discountRate":20,
         "taxRate":20,
         "taxIncluded":"Y",
         "measureCode":796,
         "sort":10,
         "type":4,
         "productName":"iphone 14",
         "priceAccount":80000,
         "priceExclusive":66666.67,
         "priceNetto":83333.34,
         "priceBrutto":100000.01,
         "discountSum":16666.67,
         "customized":"Y",
         "measureName":"pcs",
         "xmlId":""
      }
   },
   "time":{
      "start":1716887721.77879,
      "finish":1716887723.259695,
      "duration":1.4809050559997559,
      "processing":1.2986550331115723,
      "date_start":"2024-05-28T12:15:21+03:00",
      "date_finish":"2024-05-28T12:15:23+03:00"
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
[`crm_item_product_row`](../../data-types.md#crm_item_product_row) | Object containing information about the added product item ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error":"OWNER_NOT_FOUND",
   "error_description":"Owner was not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | Working with this type of objects is not supported ||
|| `ACCESS_DENIED` | Access denied ||
|| `OWNER_NOT_FOUND` | The provided CRM object was not found ||
|| `100` | Required parameters were not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-productrow-update.md)
- [{#T}](./crm-item-productrow-fields.md)
- [{#T}](./crm-item-productrow-get.md)
- [{#T}](./crm-item-productrow-set.md)
- [{#T}](./crm-item-productrow-get-available-for-payment.md)
- [{#T}](./crm-item-productrow-list.md)
- [{#T}](./crm-item-productrow-delete.md)