# Get Product Image Information v 3.0 catalog.product.image.get

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

The `catalog.product.image.get` method returns information about a product or variation image by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Image identifier.

The identifier can be retrieved using the [catalog.product.image.list](./catalog-product-image-list-rest-v3.md) method or from the response of the [catalog.product.image.add](./catalog-product-image-add-rest-v3.md) method ||
|| **productId***
[`integer`](../../data-types.md) | Identifier of the product or variation the image belongs to.

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
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.image.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":39439,"productId":7537,"select":["id","productId","type","name","detailUrl"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.image.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":39439,"productId":7537,"select":["id","productId","type","name","detailUrl"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.image.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductImageGetResult = {
      item: {
        id: number
        productId: number
        type: 'PREVIEW_PICTURE' | 'DETAIL_PICTURE' | 'MORE_PHOTO'
        name: string
        detailUrl: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductImageGetResult>({
        method: 'catalog.product.image.get',
        params: {
          id: 39439,
          productId: 7537,
          select: [
            'id',
            'productId',
            'type',
            'name',
            'detailUrl',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Image:', result.item.type, result.item.detailUrl)
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
      async function getProductImage() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.image.get',
            params: {
              id: 39439,
              productId: 7537,
              select: [
                'id',
                'productId',
                'type',
                'name',
                'detailUrl',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Image:', result.item.type, result.item.detailUrl)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getProductImage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.image.get(
            bitrix_id=39439,
            product_id=7537,
            select=["id", "productId", "type", "name", "detailUrl"],
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
                'catalog.product.image.get',
                [
                    'id' => 39439,
                    'productId' => 7537,
                    'select' => ['id', 'productId', 'type', 'name', 'detailUrl'],
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
        'catalog.product.image.get',
        {
            id: 39439,
            productId: 7537,
            select: ['id', 'productId', 'type', 'name', 'detailUrl'],
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
        'catalog.product.image.get',
        [
            'id' => 39439,
            'productId' => 7537,
            'select' => ['id', 'productId', 'type', 'name', 'detailUrl'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.image.get", b24.Params{
    	"id":        39439,
    	"productId": 7537,
    	"select":    []string{"id", "productId", "type", "name", "detailUrl"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.image.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID        int    `json:"id"`
    	Type      string `json:"type"`
    	Name      string `json:"name"`
    	DetailURL string `json:"detailUrl"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Type, item.DetailURL)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 39439,
            "productId": 7537,
            "type": "MORE_PHOTO",
            "name": "more.png",
            "detailUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/c02/c020af94aac1804f57f3a5092b8cb4b4/b3759501f5e66f5238a54d2654a7b0cc.png"
        }
    },
    "time": {
        "start": 1792660560,
        "finish": 1792660560.964274,
        "duration": 0.9642739295959473,
        "processing": 0,
        "date_start": "2026-10-22T12:16:00+02:00",
        "date_finish": "2026-10-22T12:16:00+02:00",
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
|| **result.item**
[`object`](../../data-types.md) | Image fields. The set depends on `select`. [Field description](#item) ||
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
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 1 was not found."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass a positive number in `id` ||
|| `productId` | Required field `productId` is missing | Pass a positive number in `productId` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id`, `productId` | Entity with ID `#ID#` was not found | The product or the image was not found. The message contains the identifier that was not found. Check the identifiers using the [catalog.product.image.list](./catalog-product-image-list-rest-v3.md) method ||
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
- [{#T}](./catalog-product-image-list-rest-v3.md)
- [{#T}](./catalog-product-image-delete-rest-v3.md)
- [{#T}](./catalog-product-image-field-list.md)
