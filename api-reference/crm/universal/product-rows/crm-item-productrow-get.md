# Get Information About a Product Item by ID crm.item.productrow.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires read access permission for the object to which the product items are linked.

This method retrieves information about a product item in the CRM.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`crm_item_product_row.id`](../../data-types.md#crm_item_product_row) | Identifier of the product item ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17622}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.productrow.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17622,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.productrow.get
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.item.productrow.get', {
            id: 17622,
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
        'crm.item.productrow.get',
        [
            'id' => 17622
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
         "id":17622,
         "ownerId":13141,
         "ownerType":"D",
         "productId":9621,
         "productName":"iphone 14",
         "price":79999,
         "priceAccount":79999,
         "priceExclusive":79999,
         "priceNetto":79999,
         "priceBrutto":79999,
         "quantity":11,
         "discountTypeId":2,
         "discountRate":0,
         "discountSum":0,
         "taxRate":null,
         "taxIncluded":"Y",
         "customized":"Y",
         "measureCode":796,
         "measureName":"pcs",
         "sort":10,
         "xmlId":"sale_basket_8145",
         "type":4
      }
   },
   "time":{
      "start":1716821358.26828,
      "finish":1716821358.701454,
      "duration":0.43317389488220215,
      "processing":0.240645170211792,
      "date_start":"2024-05-27T17:49:18+03:00",
      "date_finish":"2024-05-27T17:49:18+03:00"
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
[`crm_item_product_row`](../../data-types.md#crm_item_product_row) | Object containing information about the product item ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error":"NOT_FOUND",
   "error_description":"Item not found"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | Working with this type of objects is not supported ||
|| `ACCESS_DENIED` | Access denied ||
|| `NOT_FOUND` | Product item not found ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-productrow-add.md)
- [{#T}](./crm-item-productrow-update.md)
- [{#T}](./crm-item-productrow-fields.md)
- [{#T}](./crm-item-productrow-set.md)
- [{#T}](./crm-item-productrow-get-available-for-payment.md)
- [{#T}](./crm-item-productrow-list.md)
- [{#T}](./crm-item-productrow-delete.md)