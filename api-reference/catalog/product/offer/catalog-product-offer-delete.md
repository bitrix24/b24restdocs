# Delete Deal catalog.product.offer.delete

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a deal.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_offer.id`](../../data-types.md#catalog_product_offer) | Identifier of the deal.

To obtain the identifiers of deals, you need to use [catalog.product.offer.list](./catalog-product-offer-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1285}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1285,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.product.offer.delete',
        {
            id: 1285,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.product.offer.delete',
        [
            'id' => 1285
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1718623769.704759,
        "finish": 1718623770.549073,
        "duration": 0.8443140983581543,
        "processing": 0.4027719497680664,
        "date_start": "2024-06-17T14:29:29+03:00",
        "date_finish": "2024-06-17T14:29:30+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the deal deletion ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{	
    "error":200040300040,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300040` | Insufficient permissions to delete the deal
|| 
|| `200040300040` | Insufficient permissions to delete the information block
|| 
|| `200040300010` | Insufficient permissions to view the product catalog
|| 
|| `200040300000` | Information block not found
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-add.md)
- [{#T}](./catalog-product-offer-update.md)
- [{#T}](./catalog-product-offer-get.md)
- [{#T}](./catalog-product-offer-list.md)
- [{#T}](./catalog-product-offer-download.md)
- [{#T}](./catalog-product-offer-get-fields-by-filter.md)