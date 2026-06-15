# Get Information About Product Image catalog.productImage.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method returns information about a specific product image, main product, variation, or service.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **productId***
[`catalog_product.id`](../data-types.md#catalog_product)\|
[`catalog_product_sku.id`](../data-types.md#catalog_product_sku)\|
[`catalog_product_offer.id`](../data-types.md#catalog_product_offer)\|
[`catalog_product_service.id`](../data-types.md#catalog_product_service) | Identifier of the product, main product, variation, or service.

To obtain existing identifiers, use the following methods:
- for products — [catalog.product.list](../product/catalog-product-list.md)
- for main products — [catalog.product.sku.list](../product/sku/catalog-product-sku-list.md)
- for product variations — [catalog.product.offer.list](../product/offer/catalog-product-offer-list.md)
- for services — [catalog.product.service.list](../product/service/catalog-product-service-list.md)
||
|| **id***
[`catalog_product_image.id`](../data-types.md#catalog_product_image) | Identifier of the image ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"productId":1,"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productImage.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"productId":1,"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.productImage.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductImageGetResult = {
      productImage: {
        createTime: ISODate | null,
        detailUrl: string,
        downloadUrl: string,
        id: number,
        name: string,
        productId: number,
        type: string,
      },
    }

    try {
      const response = await $b24.actions.v2.call.make<ProductImageGetResult>({
        method: 'catalog.productImage.get',
        params: {
          productId: 1,
          id: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.productImage.id, result.productImage.name, result.productImage.type)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getProductImage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.productImage.get',
            params: {
              productId: 1,
              id: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.productImage.id, result.productImage.name, result.productImage.type)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getProductImage)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productImage.get',
                [
                    'productId' => 1,
                    'id'        => 1,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting product image: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productImage.get', 
        {
            productId: 1,
            id: 1,
        }, 
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.productImage.get',
        [
            'productId' => 1,
            'id' => 1
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "productImage": {
            "createTime": "2024-10-17T10:48:05+02:00",
            "detailUrl": "\/upload\/iblock\/6f1\/bkm7jmwso31wisk423gtp28iagy2e8v0\/test.jpeg",
            "downloadUrl": "http:\/\/dev.bx\/rest\/download.json?sessid=ae1ada0e5c85babd18ce4af4c702d1d9\u0026token=catalog%7CaWQ9NzY1MSZfPTZWZFhwSDRZRFRvcmNmYWtGMVRQbE4wdjZRcHA5QXBY%7CImRvd25sb2FkfGNhdGFsb2d8YVdROU56WTFNU1pmUFRaV1pGaHdTRFJaUkZSdmNtTm1ZV3RHTVZSUWJFNHdkalpSY0hBNVFYQll8YWUxYWRhMGU1Yzg1YmFiZDE4Y2U0YWY0YzcwMmQxZDki.8jeG4p%2BO6LZSDNqaRR3XdTAM6jSSD4Gtye8zm6Q5Y14%3D",
            "id": 1,
            "name": "test.jpeg",
            "productId": 1,
            "type": "MORE_PHOTO"
        }
    },
    "time": {
        "start": 1729162290.040193,
        "finish": 1729162290.63512,
        "duration": 0.5949268341064453,
        "processing": 0.20079898834228516,
        "date_start": "2024-10-17T13:51:30+02:00",
        "date_finish": "2024-10-17T13:51:30+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **productImage**
[`catalog_product_image`](../data-types.md#catalog_product_image) | Object containing information about the image ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to view the product catalog
||
|| `200040300010` | Insufficient permissions to view the product 
||
|| `100` | Parameter `productId` not specified
|| 
|| `100` | Parameter `id` not specified
|| 
|| `0` | Product with the specified identifier not found
|| 
|| `0` | Image with the specified identifier not found
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-product-image-add.md)
- [{#T}](./catalog-product-image-list.md)
- [{#T}](./catalog-product-image-delete.md)
- [{#T}](./catalog-product-image-get-fields.md)