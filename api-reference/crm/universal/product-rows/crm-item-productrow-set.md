# Save Product Row of CRM Object crm.item.productrow.set

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires permission to modify the CRM object whose product row is being set.

This method saves the product row of a CRM object. Please note that this method will overwrite all existing product rows associated with the object. Thus, it replaces the existing product rows with those that were sent.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerId***
[`integer`](../../../data-types.md) | Identifier of the CRM object ||
|| **ownerType***
[`string`](../../../data-types.md) | Identifier of the [`CRM object type`](../../data-types.md#tip-obuekta-crm) ||
|| **productRows***
[`object[]`](../../../data-types.md) | Array of objects containing information about the product rows to be saved in the object ||
|#

### Parameter productRows

#|
|| **Name**
`type` | **Description** ||
|| **productId**
[`catalog_product.id`](../../../catalog/data-types.md#catalog_product) | Identifier of the product from the catalog ||
|| **productName**
[`string`](../../../data-types.md) | Name of the product in the product row.
If not provided and **productId** is given, the product name from the product catalog will be used. ||
|| **price**
[`double`](../../../data-types.md) | Price per unit of the product row, including discounts and taxes ||
|| **quantity**
[`double`](../../../data-types.md) | Quantity of the product. 
Default is 1 ||
|| **discountTypeId**
[`integer`](../../../data-types.md) | Type of discount.
Possible values:
- `1` — absolute value
- `2` — percentage value
Default is 2 ||
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
Default is N ||
|| **measureCode**
[`catalog_measure.code`](../../../catalog/data-types.md#catalog_measure) | Unit of measure code.
If not provided and **productId** is given, the unit of measure from the product catalog will be used. ||
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
    -d '{"ownerType":"D","ownerId":13143,"productRows":[{"productId":9621,"price":99999.99,"quantity":1,"sort":10},{"productId":9623,"price":15900,"quantity":2,"sort":10}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.productrow.set
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerType":"D","ownerId":13143,"productRows":[{"productId":9621,"price":99999.99,"quantity":1,"sort":10},{"productId":9623,"price":15900,"quantity":2,"sort":10}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.productrow.set
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.item.productrow.set', {
            ownerType: 'D',
            ownerId: 13143,
            productRows: [{
                    productId: 9621,
                    price: 99999.99,
                    quantity: 1,
                    sort: 10,
                },
                {
                    productId: 9623,
                    price: 15900.00,
                    quantity: 2,
                    sort: 10,
                },

            ],
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
        'crm.item.productrow.set',
        [
            'ownerType' => 'D',
            'ownerId' => 13143,
            'productRows' => [
                [
                    'productId' => 9621,
                    'price' => 99999.99,
                    'quantity' => 1,
                    'sort' => 10,
                ],
                [
                    'productId' => 9623,
                    'price' => 15900.00,
                    'quantity' => 2,
                    'sort' => 10,
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
   "result":{
      "productRows":[
         {
            "id":17654,
            "ownerId":13143,
            "ownerType":"D",
            "productId":9621,
            "productName":"iphone 14",
            "price":99999.99,
            "priceAccount":99999.99,
            "priceExclusive":99999.99,
            "priceNetto":99999.99,
            "priceBrutto":99999.99,
            "quantity":1,
            "discountTypeId":2,
            "discountRate":0,
            "discountSum":0,
            "taxRate":null,
            "taxIncluded":"N",
            "customized":"Y",
            "measureCode":796,
            "measureName":"pcs",
            "sort":10,
            "xmlId":"",
            "type":4
         },
         {
            "id":17655,
            "ownerId":13143,
            "ownerType":"D",
            "productId":9623,
            "productName":"iphone 10xs",
            "price":15900,
            "priceAccount":15900,
            "priceExclusive":15900,
            "priceNetto":15900,
            "priceBrutto":15900,
            "quantity":2,
            "discountTypeId":2,
            "discountRate":0,
            "discountSum":0,
            "taxRate":null,
            "taxIncluded":"N",
            "customized":"Y",
            "measureCode":796,
            "measureName":"pcs",
            "sort":10,
            "xmlId":"",
            "type":4
         }
      ]
   },
   "time":{
      "start":1716895718.887229,
      "finish":1716895719.316293,
      "duration":0.4290640354156494,
      "processing":0.20114707946777344,
      "date_start":"2024-05-28T14:28:38+03:00",
      "date_finish":"2024-05-28T14:28:39+03:00"
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
[`crm_item_product_row[]`](../../data-types.md#crm_item_product_row) | Array of objects containing information about all product rows of the CRM object ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

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
- [{#T}](./crm-item-productrow-add.md)
- [{#T}](./crm-item-productrow-fields.md)
- [{#T}](./crm-item-productrow-get.md)
- [{#T}](./crm-item-productrow-update.md)
- [{#T}](./crm-item-productrow-get-available-for-payment.md)
- [{#T}](./crm-item-productrow-list.md)
- [{#T}](./crm-item-productrow-delete.md)