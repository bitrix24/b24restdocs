# Get the list of product images catalog.productImage.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of product images, parent product images, variations, or services.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **productId***
[`catalog_product.id`](../data-types.md#catalog_product)\|
[`catalog_product_sku.id`](../data-types.md#catalog_product_sku)\|
[`catalog_product_offer.id`](../data-types.md#catalog_product_offer)\|
[`catalog_product_service.id`](../data-types.md#catalog_product_service) | Identifier of the product, parent product, variation, or service.

To obtain existing identifiers, use the following methods:
- for products — [catalog.product.list](../product/catalog-product-list.md)
- for parent products — [catalog.product.sku.list](../product/sku/catalog-product-sku-list.md)
- for product variations — [catalog.product.offer.list](../product/offer/catalog-product-offer-list.md)
- for services — [catalog.product.service.list](../product/service/catalog-product-service-list.md)
||
|| **select** 
[`array`](../../data-types.md)| An array with a list of fields to select (see fields of the object [catalog_product_image](../data-types.md#catalog_product_image)) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"productId":1,"select":["id","name","productId","type","createTime","downloadUrl","detailUrl"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productImage.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"productId":1,"select":["id","name","productId","type","createTime","downloadUrl","detailUrl"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.productImage.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'catalog.productImage.list',
        {
          productId: 1,
          select: [
            'id',
            'name',
            'productId',
            'type',
            'createTime',
            'downloadUrl',
            'detailUrl',
          ],
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('catalog.productImage.list', { productId: 1, select: ['id', 'name', 'productId', 'type', 'createTime', 'downloadUrl', 'detailUrl'] }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. Suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('catalog.productImage.list', { productId: 1, select: ['id', 'name', 'productId', 'type', 'createTime', 'downloadUrl', 'detailUrl'] }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productImage.list',
                [
                    'productId' => 1,
                    'select'    => [
                        'id',
                        'name',
                        'productId',
                        'type',
                        'createTime',
                        'downloadUrl',
                        'detailUrl',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing product images: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productImage.list',
        {
            productId: 1,
            select: [
                'id',
                'name',
                'productId',
                'type',
                'createTime',
                'downloadUrl',
                'detailUrl',
            ],
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
            result.next();
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.productImage.list',
        [
            'productId' => 1,
            'select' => [
                'id',
                'name',
                'productId',
                'type',
                'createTime',
                'downloadUrl',
                'detailUrl'
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

```json
{
    "result": {
        "productImages": [
            {
                "createTime": "2024-10-17T10:48:05+02:00",
                "detailUrl": "\/upload\/iblock\/6f1\/bkm7jmwso31wisk423gtp28iagy2e8v0\/test.jpeg",
                "downloadUrl": "http:\/\/dev.bx\/rest\/download.json?sessid=ae1ada0e5c85babd18ce4af4c702d1d9\u0026token=catalog%7CaWQ9NzY1MSZfPTEzSk9hR2tKNHBRY1I2cFBPNjhvaTFHNTNiYjVsVmdx%7CImRvd25sb2FkfGNhdGFsb2d8YVdROU56WTFNU1pmUFRFelNrOWhSMnRLTkhCUlkxSTJjRkJQTmpodmFURkhOVE5pWWpWc1ZtZHh8YWUxYWRhMGU1Yzg1YmFiZDE4Y2U0YWY0YzcwMmQxZDki.iC0Yzi9feK8V1Zr0WSlp5GZpcmD0osnJGHN%2FZL%2FkQI4%3D",
                "id": 1,
                "name": "test.jpeg",
                "productId": 1,
                "type": "MORE_PHOTO"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1729163241.324569,
        "finish": 1729163241.860237,
        "duration": 0.535667896270752,
        "processing": 0.19502019882202148,
        "date_start": "2024-10-17T14:07:21+02:00",
        "date_finish": "2024-10-17T14:07:21+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **productImages**
[`catalog_product_image[]`](../data-types.md#catalog_product_image) | Array of objects with information about the selected product images ||
|| **total**
[`integer`](../../data-types.md#time) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient rights to view the trade catalog
||
|| `200040300010` | Insufficient rights to view the product
|| 
|| `100` | The `productId` parameter is not specified or is empty
|| 
|| `0` | The product with the specified identifier was not found
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Exploring 

- [{#T}](./catalog-product-image-add.md)
- [{#T}](./catalog-product-image-get.md)
- [{#T}](./catalog-product-image-delete.md)
- [{#T}](./catalog-product-image-get-fields.md)