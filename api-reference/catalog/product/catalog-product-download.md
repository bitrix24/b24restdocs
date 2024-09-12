# Download Product Files catalog.product.download

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method downloads product files from the trade catalog based on the provided parameters.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields*** 
 [`object`](../../data-types.md)| Field values for downloading product files ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fileId*** 
 [`integer`](../../data-types.md)| Identifier of the registered file.

To obtain file identifiers for products, use [catalog.product.get](./catalog-product-get.md) or [catalog.product.list](./catalog-product-list.md)
 ||
|| **productId*** 
 [`catalog_product.id`](../data-types.md#catalog_product)| Identifier of the product.

To obtain product identifiers, use [catalog.product.list](./catalog-product-list.md)
 ||
|| **fieldName*** 
[`string`](../../data-types.md) | Name of the field (property or field of the information block element) where the file is stored.

- `DETAIL_PICTURE` — detailed picture
- `PREVIEW_PICTURE` — preview picture
- `PROPERTY_N` — property, where `N` is the property identifier or code

To obtain existing identifiers or property codes for products, use [catalog.productProperty.list](../product-property/catalog-product-property-list.md)
 ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fileId":6439,"productId":1243,"fieldName":"detailPicture"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.download
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fileId":6439,"productId":1243,"fieldName":"detailPicture"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.download
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.product.download',
        {
            fields: {
                fileId: 6439,
                productId: 1243,
                fieldName: 'detailPicture',
            }
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
        'catalog.product.download',
        [
            'fields' => [
                'fileId' => 6439,
                'productId' => 1243,
                'fieldName' => 'detailPicture',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The response returns a file based on the provided parameters.

### Returned Data

A file is returned based on the provided parameters.

## Error Handling

HTTP status: **400**

```json
{	
   "error":0,
   "error_description":"Required fields: fileId"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read the trade catalog ||
|| `0` | The product with the specified identifier does not exist ||
|| `0` | The specified property does not exist or is not a file ||
|| `0` | The file with the specified identifier does not exist ||
|| `0` | Required fields were not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-product-add.md)
- [{#T}](./catalog-product-update.md)
- [{#T}](./catalog-product-get.md)
- [{#T}](./catalog-product-list.md)
- [{#T}](./catalog-product-delete.md)
- [{#T}](./catalog-product-get-fields-by-filter.md)