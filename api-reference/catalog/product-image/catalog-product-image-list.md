# Get the list of product images catalog.productImage.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View catalog products" permission and permission to read the product

The method `catalog.productImage.list` returns all images of a product, parent product, variation, or service in a single response, without pagination.

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
[`array`](../../data-types.md) | Array of fields to return:
- `id` — image identifier
- `name` — file name
- `productId` — product identifier
- `type` — image type
- `createTime` — creation date
- `downloadUrl` — download link
- `detailUrl` — link to the image

Pass field names in camelCase, as in the list. The method skips unknown fields and names in a different case, for example `ID`, without an error.

If `select` is not passed, all fields are returned ||
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductImageListResult = {
      productImages: {
        id: number
        name: string
        productId: number
        type: 'DETAIL_PICTURE' | 'PREVIEW_PICTURE' | 'MORE_PHOTO'
        createTime: ISODate
        downloadUrl: string
        detailUrl: string
      }[]
    }

    try {
      // catalog.productImage.list returns all images of the product in one response, without pagination
      const response = await $b24.actions.v2.call.make<ProductImageListResult>({
        method: 'catalog.productImage.list',
        params: {
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
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Product images:', result.productImages, 'Count:', result.productImages.length)
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
      async function listProductImages() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.productImage.list returns all images of the product in one response, without pagination
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.productImage.list',
            params: {
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
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Product images:', result.productImages, 'Count:', result.productImages.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listProductImages)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product_image.list(
            product_id=1,
            select=[
                "id",
                "name",
                "productId",
                "type",
                "createTime",
                "downloadUrl",
                "detailUrl",
            ],
            start=0,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.productImage.list", b24.Params{
    	"productId": 1,
    	"select":    []string{"id", "name", "productId", "type", "createTime", "downloadUrl", "detailUrl"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.productImage.list: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "productImages": [
            {
                "createTime": "2023-08-21T12:12:19+03:00",
                "detailUrl": "https:\/\/cdn-com.bitrix24.com\/b1234567\/iblock\/c3e\/c3e6943a5f28fb70dc0051fc74352d4f\/FTTH.jpg",
                "downloadUrl": "https:\/\/your-domain.bitrix24.com\/rest\/1\/xxxxxxxxxxxxxxxx\/download\/?token=catalog%7CaWQ9MjI0MjEmXz1nYXc0RVlHaWo2cHJZTGNCS2VBa21lQTZkdVJwZ0tpNw%3D%3D%7C...",
                "id": 22421,
                "name": "FTTH.jpg",
                "productId": 533,
                "type": "MORE_PHOTO"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1793615807,
        "finish": 1793615807.756784,
        "duration": 0.7567839622497559,
        "processing": 0,
        "date_start": "2026-11-02T13:36:47+03:00",
        "date_finish": "2026-11-02T13:36:47+03:00",
        "operating_reset_at": 1793616360,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.productImages**
[`catalog_product_image[]`](../data-types.md#catalog_product_image) | Array of product images. If there are no images, an empty array is returned ||
|| **total**
[`integer`](../../data-types.md) | Number of product images ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Product was not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user does not have the "View product catalog" or "View catalog products" permission or permission to read the product ||
|| `400` | `100` | Could not find value for parameter {productId} | The required parameter `productId` is not passed ||
|| `400` | `0` | Empty productID | `0` or a negative number is passed in `productId` ||
|| `400` | `0` | Product was not found | The product with the specified `productId` was not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Exploring

- [{#T}](./catalog-product-image-add.md)
- [{#T}](./catalog-product-image-get.md)
- [{#T}](./catalog-product-image-delete.md)
- [{#T}](./catalog-product-image-get-fields.md)
