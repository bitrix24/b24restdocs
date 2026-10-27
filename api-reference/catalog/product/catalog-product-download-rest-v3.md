# Download Product Files v 3.0 catalog.product.download

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.product.download` returns the data of a product file and a download link.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Object with the file parameters. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **productId***
[`integer`](../../data-types.md) | Product identifier.

The identifier can be retrieved using the [catalog.product.list](./catalog-product-list-rest-v3.md) method ||
|| **fieldName***
[`string`](../../data-types.md) | Product field that stores the file.

Possible values:

- `PREVIEW_PICTURE` — preview picture
- `DETAIL_PICTURE` — detail picture
- `PROPERTY_{id}` — a property of the "File" type, where `id` is the property identifier

Field names in the response format, such as `previewPicture`, are not supported ||
|| **fileId***
[`integer`](../../data-types.md) | File identifier.

The identifier is returned by the [catalog.product.get](./catalog-product-get-rest-v3.md) method: in the `previewPicture` and `detailPicture` fields or in `VALUE` of a property of the "File" type ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.download`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":7527,"fieldName":"PREVIEW_PICTURE","fileId":39429}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.download
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":7527,"fieldName":"PREVIEW_PICTURE","fileId":39429},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.download
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductDownloadResult = {
      file: {
        id: number
        fileName: string
        contentType: string
        src: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductDownloadResult>({
        method: 'catalog.product.download',
        params: {
          fields: {
            productId: 7527,
            fieldName: 'PREVIEW_PICTURE',
            fileId: 39429,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('File:', result.file.fileName, result.file.src)
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
      async function productDownload() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.download',
            params: {
              fields: {
                productId: 7527,
                fieldName: 'PREVIEW_PICTURE',
                fileId: 39429,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('File:', result.file.fileName, result.file.src)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', productDownload)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.download(
            fields={
                "productId": 7527,
                "fieldName": "PREVIEW_PICTURE",
                "fileId": 39429,
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
                'catalog.product.download',
                [
                    'fields' => [
                        'productId' => 7527,
                        'fieldName' => 'PREVIEW_PICTURE',
                        'fileId' => 39429,
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
        'catalog.product.download',
        {
            fields: {
                productId: 7527,
                fieldName: 'PREVIEW_PICTURE',
                fileId: 39429,
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
        'catalog.product.download',
        [
            'fields' => [
                'productId' => 7527,
                'fieldName' => 'PREVIEW_PICTURE',
                'fileId' => 39429,
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
    res, err := client.Core().Call(ctx, "catalog.product.download", b24.Params{
    	"fields":b24.Params{
    		"productId":7527,
    		"fieldName":"PREVIEW_PICTURE",
    		"fileId":   39429,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.download: %w", err)
    }

    var result struct {
    	File struct {
    		ID          int    `json:"id"`
    		FileName    string `json:"fileName"`
    		ContentType string `json:"contentType"`
    		Src         string `json:"src"`
    	} `json:"file"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.File.FileName, result.File.Src)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "file": {
            "id": 39429,
            "fileName": "tshirt-preview.png",
            "contentType": "image/png",
            "src": "https://cdn-com.bitrix24.com/b1234567/iblock/cf3/cf383e376005a22cf23e42dab0e1506a/tshirt-preview.png"
        }
    },
    "time": {
        "start": 1792660671,
        "finish": 1792660671.742098,
        "duration": 0.7420980930328369,
        "processing": 0,
        "date_start": "2026-10-22T12:17:51+02:00",
        "date_finish": "2026-10-22T12:17:51+02:00",
        "operating_reset_at": 1792661220,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.file**
[`object`](../../data-types.md) | File data. [Field description](#file) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### File Object {#file}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | File identifier ||
|| **fileName**
[`string`](../../data-types.md) | File name ||
|| **contentType**
[`string`](../../data-types.md) | MIME type of the file, for example `image/png` ||
|| **src**
[`string`](../../data-types.md) | Link to download the file ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Name file field is not available"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the file parameters in the `fields` object ||
|| `-` | Name file field is not available | Pass `PREVIEW_PICTURE`, `DETAIL_PICTURE`, or `PROPERTY_{id}` of a property of the "File" type in `fieldName` ||
|| `-` | Product file wrong | The file does not belong to the product field. Check `fileId` using the [catalog.product.get](./catalog-product-get-rest-v3.md) method ||
|| `-` | Product is empty | The file was not found in Bitrix24. Check `fileId` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `productId` | Entity with ID `#ID#` was not found | Check the product identifier using the [catalog.product.list](./catalog-product-list-rest-v3.md) method ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions to view the catalog and to read the information block element ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-get-rest-v3.md)
- [{#T}](./catalog-product-update-rest-v3.md)
