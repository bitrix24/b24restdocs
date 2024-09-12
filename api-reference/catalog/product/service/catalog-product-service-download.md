# Download Service Files catalog.product.service.download

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method downloads service files based on the provided parameters.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values for downloading service files ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fileId***
[`integer`](../../../data-types.md) | Identifier of the registered file.

To obtain the identifiers of service files, you need to use [catalog.product.service.get](./catalog-product-service-get.md) or [catalog.product.service.list](./catalog-product-service-list.md)
||
|| **productId***
[`catalog_product_service.id`](../../data-types.md#catalog_product_service) | Identifier of the service.

To obtain the identifiers of the service, you need to use [catalog.product.service.list](./catalog-product-service-list.md)
||
|| **fieldName***
[`string`](../../../data-types.md) | The name of the field (property or field of the information block element) where the file is stored. Possible values:
- `DETAIL_PICTURE` — detailed picture
- `PREVIEW_PICTURE` — preview picture
- `PROPERTY_N` — property, where `N` is the property identifier or code

To obtain existing identifiers or property codes of services, you need to use [catalog.productProperty.list](../../product-property/catalog-product-property-list.md)
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
    -d '{"fields":{"fileId":6497,"productId":1265,"fieldName":"detailPicture"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.download
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fileId":6497,"productId":1265,"fieldName":"detailPicture"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.service.download
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.product.service.download',
        {
            fields: {
                fileId: 6497,
                productId: 1265,
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
        'catalog.product.service.download',
        [
            'fields' => [
                'fileId' => 6497,
                'productId' => 1265,
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

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read the trade catalog
|| 
|| `0` | The service with the specified identifier does not exist
|| 
|| `0` | The specified property does not exist or is not a file
|| 
|| `0` | The file with the specified identifier does not exist
|| 
|| `0` | Required fields are not provided
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-add.md)
- [{#T}](./catalog-product-service-update.md)
- [{#T}](./catalog-product-service-get.md)
- [{#T}](./catalog-product-service-list.md)
- [{#T}](./catalog-product-service-delete.md)
- [{#T}](./catalog-product-service-get-fields-by-filter.md)