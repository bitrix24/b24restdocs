# Download Product Variation Files v 3.0 catalog.product.offer.download

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `catalog.product.offer.download` returns variation file data: the name, type, and file link.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with file parameters. [Field description](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **productId***
[`integer`](../../../data-types.md) | Variation identifier.

The identifier can be retrieved using the [catalog.product.offer.list](./catalog-product-offer-list-rest-v3.md) method or from the [catalog.product.offer.add](./catalog-product-offer-add-rest-v3.md) response ||
|| **fieldName***
[`string`](../../../data-types.md) | Name of the field that stores the file, in upper case with underscores.

Possible values:

- `PREVIEW_PICTURE` — preview picture
- `DETAIL_PICTURE` — detail picture
- `PROPERTY_{id}` — a property of the "File" type

Names in the `detailPicture` format are not supported ||
|| **fileId***
[`integer`](../../../data-types.md) | File identifier. It is returned by the [catalog.product.offer.get](./catalog-product-offer-get-rest-v3.md) method in the `previewPicture` and `detailPicture` fields or in the value of a file property in `properties` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.offer.download`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":7555,"fieldName":"DETAIL_PICTURE","fileId":39457}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.download
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":7555,"fieldName":"DETAIL_PICTURE","fileId":39457},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.offer.download
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DownloadResult = {
      file: {
        id: number
        fileName: string
        contentType: string
        src: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<DownloadResult>({
        method: 'catalog.product.offer.download',
        params: {
          fields: {
            productId: 7555,
            fieldName: 'DETAIL_PICTURE',
            fileId: 39457,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('File:', result.file.src)
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
      async function downloadOfferFile() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.offer.download',
            params: {
              fields: {
                productId: 7555,
                fieldName: 'DETAIL_PICTURE',
                fileId: 39457,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('File:', result.file.src)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', downloadOfferFile)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.offer.download(
            fields={
                "productId": 7555,
                "fieldName": "DETAIL_PICTURE",
                "fileId": 39457,
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
                'catalog.product.offer.download',
                [
                    'fields' => [
                        'productId' => 7555,
                        'fieldName' => 'DETAIL_PICTURE',
                        'fileId' => 39457,
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
        'catalog.product.offer.download',
        {
            fields: {
                productId: 7555,
                fieldName: 'DETAIL_PICTURE',
                fileId: 39457,
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
        'catalog.product.offer.download',
        [
            'fields' => [
                'productId' => 7555,
                'fieldName' => 'DETAIL_PICTURE',
                'fileId' => 39457,
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
    res, err := client.Core().Call(ctx, "catalog.product.offer.download", b24.Params{
    	"fields": b24.Params{
    		"productId": 7555,
    		"fieldName": "DETAIL_PICTURE",
    		"fileId": 39457,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.offer.download: %w", err)
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
    fmt.Println(result.File.Src)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "file": {
            "id": 39457,
            "fileName": "futbolka.png",
            "contentType": "image/png",
            "src": "https://cdn-com.bitrix24.com/b1234567/iblock/f54/f54f7f571d4625527e27ec2317404c3b/futbolka.png"
        }
    },
    "time": {
        "start": 1792660771,
        "finish": 1792660771.310511,
        "duration": 0.31051111221313477,
        "processing": 0,
        "date_start": "2026-10-22T12:19:31+02:00",
        "date_finish": "2026-10-22T12:19:31+02:00",
        "operating_reset_at": 1792661340,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Response data object ||
|| **result.file**
[`object`](../../../data-types.md) | File data ||
|| **result.file.id**
[`integer`](../../../data-types.md) | File identifier ||
|| **result.file.fileName**
[`string`](../../../data-types.md) | File name ||
|| **result.file.contentType**
[`string`](../../../data-types.md) | File MIME type, for example `image/png` ||
|| **result.file.src**
[`string`](../../../data-types.md) | File link ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `-` | Name file field is not available | Pass `PREVIEW_PICTURE`, `DETAIL_PICTURE`, or `PROPERTY_{id}` of a "File" type property in `fieldName` ||
|| `-` | Product file wrong | The `fieldName` field has no file with this `fileId`. Check the value using the [catalog.product.offer.get](./catalog-product-offer-get-rest-v3.md) method ||
|| `-` | Product is empty | The file is not found. Retrieve the current file identifier ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | The record does not exist or is not a variation. Check `productId` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions for the catalog and the information block ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-add-rest-v3.md)
- [{#T}](./catalog-product-offer-update-rest-v3.md)
- [{#T}](./catalog-product-offer-get-rest-v3.md)
- [{#T}](./catalog-product-offer-list-rest-v3.md)
- [{#T}](./catalog-product-offer-delete-rest-v3.md)
- [{#T}](./catalog-product-offer-field-list.md)
- [{#T}](./catalog-product-offer-field-get.md)
