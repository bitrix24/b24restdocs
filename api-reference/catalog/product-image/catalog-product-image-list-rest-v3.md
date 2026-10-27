# Get a List of Product Images v 3.0 catalog.product.image.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.image.list` method returns the images of one product or variation.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../data-types.md) | A filter in REST 3.0 format: an array of conditions in the form `["field", "operator", "value"]`. The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter).

The filter must contain a condition on `productId` with the `=` operator, for example `[["productId","=",7537]]`. The method does not support other fields and operators.

The product identifier can be retrieved using the [catalog.product.list](../product/catalog-product-list-rest-v3.md) method, the variation identifier — using the [catalog.product.offer.list](../product/offer/catalog-product-offer-list-rest-v3.md) method ||
|| **select**
[`array`](../../data-types.md) | A list of image fields to return in the response.

Available fields:

- `id` — image identifier
- `productId` — product or variation identifier
- `type` — image type
- `name` — file name
- `createTime` — file upload date and time
- `detailUrl` — view link
- `downloadUrl` — download link

Default: all fields. An empty array `[]` also returns all fields ||
|| **pagination**
[`object`](../../data-types.md) | Pagination. [Object description](#pagination) ||
|#

Sorting with `order` is not supported. Images are returned in this order: preview picture, detail picture, additional pictures.

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | The number of images per page. The value `0` causes an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1` ||
|| **offset**
[`integer`](../../data-types.md) | The number of images to skip ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.image.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["productId","=",7537]]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.image.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["productId","=",7537]],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.image.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductImageListResult = {
      items: Array<{
        id: number
        productId: number
        type: 'PREVIEW_PICTURE' | 'DETAIL_PICTURE' | 'MORE_PHOTO'
        name: string
        createTime: ISODate
        detailUrl: string
        downloadUrl: string
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductImageListResult>({
        method: 'catalog.product.image.list',
        params: {
          filter: [
            ['productId', '=', 7537],
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(image => console.info(image.id, image.type, image.detailUrl))
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
      async function listProductImages() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.image.list',
            params: {
              filter: [
                ['productId', '=', 7537],
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(image => console.info(image.id, image.type, image.detailUrl))
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
        bitrix_response = client.catalog.product.image.list(
            filter=[
                ["productId", "=", 7537],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.product.image.list',
                [
                    'filter' => [
                        ['productId', '=', 7537],
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
        'catalog.product.image.list',
        {
            filter: [
                ['productId', '=', 7537],
            ],
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
        'catalog.product.image.list',
        [
            'filter' => [
                ['productId', '=', 7537],
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
    res, err := client.Core().Call(ctx, "catalog.product.image.list", b24.Params{
    	"filter": []any{
    		[]any{"productId", "=", 7537},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.image.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID        int    `json:"id"`
    		Type      string `json:"type"`
    		Name      string `json:"name"`
    		DetailURL string `json:"detailUrl"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, image := range result.Items {
    	fmt.Println(image.ID, image.Type, image.DetailURL)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 39443,
                "productId": 7537,
                "type": "PREVIEW_PICTURE",
                "name": "preview.png",
                "createTime": "2026-10-22T12:14:32+02:00",
                "detailUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/c02/c020af94aac1804f57f3a5092b8cb4b4/b3759501f5e66f5238a54d2654a7b0cc.png",
                "downloadUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/c02/c020af94aac1804f57f3a5092b8cb4b4/b3759501f5e66f5238a54d2654a7b0cc.png"
            },
            {
                "id": 39447,
                "productId": 7537,
                "type": "DETAIL_PICTURE",
                "name": "red2.png",
                "createTime": "2026-10-22T12:14:34+02:00",
                "detailUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/c02/c020af94aac1804f57f3a5092b8cb4b4/b3759501f5e66f5238a54d2654a7b0cc.png",
                "downloadUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/c02/c020af94aac1804f57f3a5092b8cb4b4/b3759501f5e66f5238a54d2654a7b0cc.png"
            },
            {
                "id": 39439,
                "productId": 7537,
                "type": "MORE_PHOTO",
                "name": "more.png",
                "createTime": "2026-10-22T12:14:20+02:00",
                "detailUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/c02/c020af94aac1804f57f3a5092b8cb4b4/b3759501f5e66f5238a54d2654a7b0cc.png",
                "downloadUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/c02/c020af94aac1804f57f3a5092b8cb4b4/b3759501f5e66f5238a54d2654a7b0cc.png"
            },
            {
                "id": 39451,
                "productId": 7537,
                "type": "MORE_PHOTO",
                "name": "photo.png",
                "createTime": "2026-10-22T12:15:59+02:00",
                "detailUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/c02/c020af94aac1804f57f3a5092b8cb4b4/b3759501f5e66f5238a54d2654a7b0cc.png",
                "downloadUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/c02/c020af94aac1804f57f3a5092b8cb4b4/b3759501f5e66f5238a54d2654a7b0cc.png"
            }
        ]
    },
    "time": {
        "start": 1792660562,
        "finish": 1792660562.218583,
        "duration": 0.2185831069946289,
        "processing": 0,
        "date_start": "2026-10-22T12:16:02+02:00",
        "date_finish": "2026-10-22T12:16:02+02:00",
        "operating_reset_at": 1792661160,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Response data object ||
|| **result.items**
[`array`](../../data-types.md) | An array of images. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

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
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION",
        "message": "Error validating request object filters.",
        "validation": [
            {
                "message": "Filter for required field \"productId\" is required.",
                "field": "filter.productId"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Filter Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.productId` | Filter for required field `productId` is required | Pass the condition `["productId","=",<identifier>]` with the `=` operator in `filter` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Field value must not be empty | Pass `filter` with a condition on `productId` ||
|| `#FIELD#` | DTO `ProductImageDto` in field `#FIELD#` requires attribute `Filterable` to perform this request | Filter only by `productId` ||
|| `id` | DTO `ProductImageDto` in field `id` requires attribute `Sortable` to perform this request | Remove `order` from the request: sorting is not supported ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass a number greater than `0` in `limit` ||
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
|| `select` | Unknown field `#FIELD#` found in entity `ProductImageDto` | Specify in `select` only the fields from the `select` parameter description ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions to view the catalog and to read the product in the information block ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-image-add-rest-v3.md)
- [{#T}](./catalog-product-image-get-rest-v3.md)
- [{#T}](./catalog-product-image-delete-rest-v3.md)
- [{#T}](./catalog-product-image-field-list.md)
