# Add an Image to a Product v 3.0 catalog.product.image.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit product" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.image.add` method uploads an image and adds it to a product or variation.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object with the fields of the new image. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **productId***
[`integer`](../../data-types.md) | Product or variation identifier.

The product identifier can be retrieved using the [catalog.product.list](../product/catalog-product-list-rest-v3.md) method, the variation identifier — using the [catalog.product.offer.list](../product/offer/catalog-product-offer-list-rest-v3.md) method ||
|| **type***
[`string`](../../data-types.md) | Where to add the image.

Possible values:

- `PREVIEW_PICTURE` — preview picture. Replaces the current preview picture
- `DETAIL_PICTURE` — detail picture. Replaces the current detail picture
- `MORE_PHOTO` — additional picture. Added to the pictures already uploaded ||
|| **file***
[`object`](../../data-types.md) | Image file. [Object description](#file) ||
|#

### Parameter file {#file}

#|
|| **Name**
`type` | **Description** ||
|| **upload***
[`object`](../../data-types.md) | Data of the file being uploaded. [Object description](#upload) ||
|#

### Object file.upload {#upload}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | File name with an extension, for example `photo.png`. The file type is checked by the extension: only an image can be uploaded ||
|| **data**
[`string`](../../data-types.md) | File content in Base64 format.

Pass either `data` or `url`, not both ||
|| **url**
[`string`](../../data-types.md) | A public link to the file. Bitrix24 downloads the file from the link.

Pass either `data` or `url`, not both ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.image.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":7537,"type":"MORE_PHOTO","file":{"upload":{"name":"photo.png","data":"iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAAAFklEQVR4nGO4o6FBEmIY1TCqYfhqAAAyBCwQhvh37QAAAABJRU5ErkJggg=="}}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.image.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":7537,"type":"MORE_PHOTO","file":{"upload":{"name":"photo.png","data":"iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAAAFklEQVR4nGO4o6FBEmIY1TCqYfhqAAAyBCwQhvh37QAAAABJRU5ErkJggg=="}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.image.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductImageAddResult = {
      item: {
        id: number
        productId: number
        type: 'PREVIEW_PICTURE' | 'DETAIL_PICTURE' | 'MORE_PHOTO'
        name: string
        createTime: ISODate
        detailUrl: string
        downloadUrl: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductImageAddResult>({
        method: 'catalog.product.image.add',
        params: {
          fields: {
            productId: 7537,
            type: 'MORE_PHOTO',
            file: {
              upload: {
                name: 'photo.png',
                data: 'iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAAAFklEQVR4nGO4o6FBEmIY1TCqYfhqAAAyBCwQhvh37QAAAABJRU5ErkJggg==',
              },
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Image added:', result.item.id, result.item.detailUrl)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addProductImage() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.image.add',
            params: {
              fields: {
                productId: 7537,
                type: 'MORE_PHOTO',
                file: {
                  upload: {
                    name: 'photo.png',
                    data: 'iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAAAFklEQVR4nGO4o6FBEmIY1TCqYfhqAAAyBCwQhvh37QAAAABJRU5ErkJggg==',
                  },
                },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Image added:', result.item.id, result.item.detailUrl)
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
        bitrix_response = client.catalog.product.image.add(
            fields={
                "productId": 7537,
                "type": "MORE_PHOTO",
                "file": {
                    "upload": {
                        "name": "photo.png",
                        "data": "iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAAAFklEQVR4nGO4o6FBEmIY1TCqYfhqAAAyBCwQhvh37QAAAABJRU5ErkJggg==",
                    },
                },
            },
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.product.image.add',
                [
                    'fields' => [
                        'productId' => 7537,
                        'type' => 'MORE_PHOTO',
                        'file' => [
                            'upload' => [
                                'name' => 'photo.png',
                                'data' => base64_encode(file_get_contents('photo.png')),
                            ],
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.product.image.add',
        {
            fields: {
                productId: 7537,
                type: 'MORE_PHOTO',
                file: {
                    upload: {
                        name: 'photo.png',
                        data: 'iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAAAFklEQVR4nGO4o6FBEmIY1TCqYfhqAAAyBCwQhvh37QAAAABJRU5ErkJggg==',
                    },
                },
            },
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.product.image.add',
        [
            'fields' => [
                'productId' => 7537,
                'type' => 'MORE_PHOTO',
                'file' => [
                    'upload' => [
                        'name' => 'photo.png',
                        'data' => base64_encode(file_get_contents('photo.png')),
                    ],
                ],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.image.add", b24.Params{
    	"fields": b24.Params{
    		"productId": 7537,
    		"type":      "MORE_PHOTO",
    		"file": b24.Params{
    			"upload": b24.Params{
    				"name": "photo.png",
    				"data": "iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAAAFklEQVR4nGO4o6FBEmIY1TCqYfhqAAAyBCwQhvh37QAAAABJRU5ErkJggg==",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.image.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID        int    `json:"id"`
    	ProductID int    `json:"productId"`
    	Type      string `json:"type"`
    	DetailURL string `json:"detailUrl"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.DetailURL)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 39451,
            "productId": 7537,
            "type": "MORE_PHOTO",
            "name": "photo.png",
            "createTime": "2026-10-22T12:15:59+02:00",
            "detailUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/c02/c020af94aac1804f57f3a5092b8cb4b4/b3759501f5e66f5238a54d2654a7b0cc.png",
            "downloadUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/c02/c020af94aac1804f57f3a5092b8cb4b4/b3759501f5e66f5238a54d2654a7b0cc.png"
        }
    },
    "time": {
        "start": 1792660559,
        "finish": 1792660559.878817,
        "duration": 0.878817081451416,
        "processing": 0,
        "date_start": "2026-10-22T12:15:59+02:00",
        "date_finish": "2026-10-22T12:15:59+02:00",
        "operating_reset_at": 1792661040,
        "operating": 4.06761908531189
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Response data object ||
|| **result.item**
[`object`](../../data-types.md) | Fields of the added image. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Image identifier. Matches the file identifier in Bitrix24 ||
|| **productId**
[`integer`](../../data-types.md) | Product or variation identifier ||
|| **type**
[`string`](../../data-types.md) | Image type: `PREVIEW_PICTURE` — preview picture, `DETAIL_PICTURE` — detail picture, `MORE_PHOTO` — additional picture ||
|| **name**
[`string`](../../data-types.md) | File name ||
|| **createTime**
[`datetime`](../../data-types.md) | File upload date and time ||
|| **detailUrl**
[`string`](../../data-types.md) | Link to view the image ||
|| **downloadUrl**
[`string`](../../data-types.md) | Link to download the image ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_INVALIDFILEEXCEPTION",
        "message": "REST_V3_EXCEPTION_VALIDATION_INVALIDFILEEXCEPTION",
        "validation": [
            {
                "message": "File of invalid type.",
                "field": "FILE_TYPE_NOT_ALLOWED"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `productId` | Required field `productId` is empty | Pass `productId` in `fields` ||
|| `type` | Required field `type` is empty | Pass `type` in `fields` ||
|| `file` | Required field `file` is empty | Pass the `file` object with the nested `upload` object ||
|| `file.upload` | At least one of the required fields must be not empty | Pass the `data` or `url` parameter in `upload` ||
|| `file.upload` | Only one of the following values must be provided: data, url | Keep only `data` or only `url` in `upload` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `type` | This request requires data type `ProductImageType` in field `type` | Pass `PREVIEW_PICTURE`, `DETAIL_PICTURE`, or `MORE_PHOTO` ||
|| `-` | Image product property does not exists. Create MORE_PHOTO property | The catalog has no `MORE_PHOTO` property. Add the image to `PREVIEW_PICTURE` or `DETAIL_PICTURE` ||
|| `file` | Could not save image. | Repeat the request ||
|#

#### File Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_INVALIDFILEEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `FILE_TYPE_NOT_ALLOWED` | File of invalid type | Upload an image. Specify a file name with an image extension in `name`, for example `photo.png` ||
|| `-` | Service response status code is 404 | The file at the `url` link is unavailable. Make sure the link opens without authorization ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `productId` | Entity with ID `#ID#` was not found | The product was not found or does not belong to the trade catalog. Check the identifier using the [catalog.product.list](../product/catalog-product-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductImageDto` | Pass only `productId`, `type`, and `file` in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "Edit product" permission and permission to edit the product in the information block ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-image-get-rest-v3.md)
- [{#T}](./catalog-product-image-list-rest-v3.md)
- [{#T}](./catalog-product-image-delete-rest-v3.md)
- [{#T}](./catalog-product-image-field-list.md)
