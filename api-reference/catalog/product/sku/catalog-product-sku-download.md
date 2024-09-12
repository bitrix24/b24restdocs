# Download Head Product Files catalog.product.sku.download

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method downloads head product files based on the provided parameters.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values for downloading head product files ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fileId***
[`integer`](../../../data-types.md) | Identifier of the registered file.

To obtain the identifiers of head product files, use [catalog.product.sku.get](./catalog-product-sku-get.md) or [catalog.product.sku.list](./catalog-product-sku-list.md)
||
|| **productId***
[`catalog_product_sku.id`](../../data-types.md#catalog_product_sku) | Identifier of the head product.

To obtain the identifiers of head products, use [catalog.product.sku.list](./catalog-product-sku-list.md)
||
|| **fieldName***
[`string`](../../../data-types.md) | Name of the field (property or field of the information block element) where the file is stored. Possible values:
- `DETAIL_PICTURE` — detailed picture
- `PREVIEW_PICTURE` — preview picture
- `PROPERTY_N` — property, where `N` is the property identifier or code

To obtain existing identifiers or codes of properties for head products, use [catalog.productProperty.list](../../product-property/catalog-product-property-list.md)
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fileId":6546,"productId":1289,"fieldName":"detailPicture"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.download
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fileId":6546,"productId":1289,"fieldName":"detailPicture"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.sku.download
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.product.sku.download',
        {
            fields: {
                fileId: 6546,
                productId: 1289,
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
        'catalog.product.sku.download',
        [
            'fields' => [
                'fileId' => 6546,
                'productId' => 1289,
                'fieldName' => 'detailPicture'
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

A file is returned based on the provided parameters.

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

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read the trade catalog
|| 
|| `0` | Head product with the specified identifier does not exist
|| 
|| `0` | The specified property does not exist or is not a file
|| 
|| `0` | File with the specified identifier does not exist
|| 
|| `0` | Required fields are not provided
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-sku-add.md)
- [{#T}](./catalog-product-sku-update.md)
- [{#T}](./catalog-product-sku-get.md)
- [{#T}](./catalog-product-sku-list.md)
- [{#T}](./catalog-product-sku-delete.md)
- [{#T}](./catalog-product-sku-get-fields-by-filter.md)