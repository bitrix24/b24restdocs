# Add Image to Product catalog.productImage.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View catalog products" permission and permission to edit the product

The method `catalog.productImage.add` adds an image to a product, parent product, variation, or service.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Object with the fields of the new image [(detailed description)](#fields) ||
|| **fileContent***
[`array`](../../data-types.md) | Array of two elements:
- the first is the file name with an extension, for example `photo.jpg`
- the second is the file content in [Base64](../../files/how-to-upload-files.md) encoding

The method checks the file type by the extension in the name: only image extensions are accepted ||
|#

### Parameter fields {#fields}

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
|| **type**
[`string`](../../data-types.md) | Image type:
- `DETAIL_PICTURE` — detail image, the field is available in the old product card. Replaces the current detail image
- `PREVIEW_PICTURE` — preview image, the field is available in the old product card. Replaces the current preview image
- `MORE_PHOTO` — additional image. Added to the images already uploaded

Default: `MORE_PHOTO`. The method also saves any other value as `MORE_PHOTO` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":1,"type":"MORE_PHOTO"},"fileContent":["photo.png","iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR4nGP4z8DwHwAFAAH/iZk9HQAAAABJRU5ErkJggg=="]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productImage.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":1,"type":"MORE_PHOTO"},"fileContent":["photo.png","iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR4nGP4z8DwHwAFAAH/iZk9HQAAAABJRU5ErkJggg=="],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.productImage.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductImageAddResult = {
      productImage: {
        createTime: ISODate,
        detailUrl: string,
        downloadUrl: string,
        id: number,
        name: string,
        productId: number,
        type: 'DETAIL_PICTURE' | 'PREVIEW_PICTURE' | 'MORE_PHOTO',
      },
    }

    try {
      const response = await $b24.actions.v2.call.make<ProductImageAddResult>({
        method: 'catalog.productImage.add',
        params: {
          fields: {
            productId: 1,
            type: 'MORE_PHOTO',
          },
          fileContent: [
            'photo.png',
            'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR4nGP4z8DwHwAFAAH/iZk9HQAAAABJRU5ErkJggg==',
          ],
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
      async function addProductImage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.productImage.add',
            params: {
              fields: {
                productId: 1,
                type: 'MORE_PHOTO',
              },
              fileContent: [
                'photo.png',
                'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR4nGP4z8DwHwAFAAH/iZk9HQAAAABJRU5ErkJggg==',
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
          console.info(result.productImage.id, result.productImage.name, result.productImage.type)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addProductImage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product_image.add(
            fields={
                "productId": 1,
                "type": "MORE_PHOTO",
            },
            file_content=[
                "photo.png",
                "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR4nGP4z8DwHwAFAAH/iZk9HQAAAABJRU5ErkJggg==",
            ],
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
                'catalog.productImage.add',
                [
                    'fields' => [
                        'productId' => 1,
                        'type' => 'MORE_PHOTO',
                    ],
                    'fileContent' => [
                        'photo.png',
                        'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR4nGP4z8DwHwAFAAH/iZk9HQAAAABJRU5ErkJggg==',
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
        echo 'Error adding product image: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'catalog.productImage.add',
    {
    'fields': {
    'productId': 1,
    'type': 'MORE_PHOTO'
    },
    'fileContent': [
    'photo.png',
    'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR4nGP4z8DwHwAFAAH/iZk9HQAAAABJRU5ErkJggg=='
    ]
    },
    function(result) {
    if (result.error())
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
        'catalog.productImage.add',
        [
            'fields' => [
                'productId' => 1,
                'type' => 'MORE_PHOTO'
            ],
            'fileContent' => [
                'photo.png',
                'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR4nGP4z8DwHwAFAAH/iZk9HQAAAABJRU5ErkJggg=='
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
    res, err := client.Core().Call(ctx, "catalog.productImage.add", b24.Params{
    	"fields": b24.Params{
    		"productId": 1,
    		"type":      "MORE_PHOTO",
    	},
    	"fileContent": []string{"photo.png", "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR4nGP4z8DwHwAFAAH/iZk9HQAAAABJRU5ErkJggg=="},
    })
    if err != nil {
    	return fmt.Errorf("catalog.productImage.add: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "productImage": {
            "createTime": "2026-11-02T14:23:05+03:00",
            "detailUrl": "https:\/\/cdn-com.bitrix24.com\/b1234567\/iblock\/26e\/26e2cde359418039bc36dc25c6c25c88\/photo.png",
            "downloadUrl": "https:\/\/your-domain.bitrix24.com\/rest\/1\/xxxxxxxxxxxxxxxx\/download\/?token=catalog%7CaWQ9Mzk4NjcmXz1DV3JqWXptcVExY0JxaDVDQWJQa2ZoT29Bak9VSmRWZQ%3D%3D%7C...",
            "id": 39867,
            "name": "photo.png",
            "productId": 533,
            "type": "MORE_PHOTO"
        }
    },
    "time": {
        "start": 1793618585,
        "finish": 1793618585.566613,
        "duration": 0.566612958908081,
        "processing": 0,
        "date_start": "2026-11-02T14:23:05+03:00",
        "date_finish": "2026-11-02T14:23:05+03:00",
        "operating_reset_at": 1793619180,
        "operating": 0.4662008285522461
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.productImage**
[`catalog_product_image`](../data-types.md#catalog_product_image) | Object with information about the added product image ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `400` | `200040300020` | Access Denied | The user does not have the "View catalog products" permission or permission to edit the product ||
|| `400` | `100` | Could not find value for parameter {fields} | The required parameter `fields` is not passed ||
|| `400` | `100` | Could not find value for parameter {fileContent} | The required parameter `fileContent` is not passed ||
|| `400` | `0` | Required fields: productId | The `productId` field is not passed in `fields` ||
|| `400` | `0` | Product was not found | The product with the specified `productId` was not found ||
|| `400` | `0` | Could not save image. | `fileContent` does not contain the file name or content ||
|| `400` | `0` | Invalid file type (txt) | The extension in the file name is not an image extension, or the name is empty. The passed extension is shown in parentheses ||
|| `400` | `0` | The filename contains invalid characters. | The file name contains invalid characters ||
|| `400` | `0` | Filename is too long. | The file name exceeds the allowed length ||
|| `400` | `0` | Maximum allowed size of the file to upload exceeded | The file size exceeds the Bitrix24 limit ||
|| `400` | `0` | Disk quota exceeded. | There is not enough space on the Bitrix24 disk ||
|| `400` | `0` | Image product property does not exists. Create MORE_PHOTO property | The catalog does not have a property with the `MORE_PHOTO` code for additional images ||
|| `400` | `0` | Image product property does not active. Activate MORE_PHOTO property | The `MORE_PHOTO` property is inactive ||
|| `400` | `0` | Image product property is of the wrong type | The `MORE_PHOTO` property is not a "File" property ||
|| `400` | `0` | Product image was not saved. | The preview image or detail image was not saved in the product, the changes were rolled back ||
|| `400` | `0` | Internal error adding product image. Try adding again. | Database error while saving. Repeat the request ||
|| `400` | `0` | Empty image. | After saving, no additional images were found for the product ||
|#

File validation error messages are returned in the Bitrix24 interface language.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-image-get.md)
- [{#T}](./catalog-product-image-list.md)
- [{#T}](./catalog-product-image-delete.md)
- [{#T}](./catalog-product-image-get-fields.md)
