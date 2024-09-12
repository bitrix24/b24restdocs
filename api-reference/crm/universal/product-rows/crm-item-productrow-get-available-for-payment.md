# Get Unpaid Product Items of CRM Object crm.item.productrow.getAvailableForPayment

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires read access permission for the CRM object whose product items are being selected.

This method retrieves the product items of a CRM object for which the client has not yet been billed.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerId***
[`integer`](../../../data-types.md) | Identifier of the CRM object ||
|| **ownerType***
[`string`](../../../data-types.md) | Identifier of the [`CRM object type`](../../data-types.md#crm-object-type) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerType":"D","ownerId":13144}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.productrow.getAvailableForPayment
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerType":"D","ownerId":13144,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.productrow.getAvailableForPayment
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.item.productrow.getAvailableForPayment', {
            ownerType: 'D',
            ownerId: 13144,
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
        'crm.item.productrow.getAvailableForPayment',
        [
            'ownerType' => 'D',
            'ownerId' => 13144
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
      "productRows":[
         {
            "id":17657,
            "ownerId":13144,
            "ownerType":"D",
            "productId":9621,
            "productName":"iphone 14",
            "price":79999,
            "priceAccount":79999,
            "priceExclusive":79999,
            "priceNetto":79999,
            "priceBrutto":79999,
            "quantity":3,
            "discountTypeId":2,
            "discountRate":0,
            "discountSum":0,
            "taxRate":null,
            "taxIncluded":"Y",
            "customized":"Y",
            "measureCode":796,
            "measureName":"pcs",
            "sort":10,
            "xmlId":"sale_basket_8149",
            "type":4
         },
         {
            "id":17658,
            "ownerId":13144,
            "ownerType":"D",
            "productId":9623,
            "productName":"iphone 10xs",
            "price":5550,
            "priceAccount":5550,
            "priceExclusive":5550,
            "priceNetto":5550,
            "priceBrutto":5550,
            "quantity":3,
            "discountTypeId":2,
            "discountRate":0,
            "discountSum":0,
            "taxRate":null,
            "taxIncluded":"Y",
            "customized":"Y",
            "measureCode":796,
            "measureName":"pcs",
            "sort":20,
            "xmlId":"sale_basket_8150",
            "type":4
         }
      ]
   },
   "time":{
      "start":1716966560.3205,
      "finish":1716966560.742781,
      "duration":0.42228102684020996,
      "processing":0.17676782608032227,
      "date_start":"2024-05-29T10:09:20+03:00",
      "date_finish":"2024-05-29T10:09:20+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **productRows**
[`crm_item_product_row[]`](../../data-types.md#crm_item_product_row) | Array of objects containing information about all product items of the CRM object for which the client has not yet been billed ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error":"ACCESS_DENIED",
   "error_description":"Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Access denied ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-productrow-add.md)
- [{#T}](./crm-item-productrow-fields.md)
- [{#T}](./crm-item-productrow-get.md)
- [{#T}](./crm-item-productrow-set.md)
- [{#T}](./crm-item-productrow-update.md)
- [{#T}](./crm-item-productrow-list.md)
- [{#T}](./crm-item-productrow-delete.md)