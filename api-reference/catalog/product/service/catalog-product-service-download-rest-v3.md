# Download Service Files v 3.0 catalog.product.service.download

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

The `catalog.product.service.download` method returns the data of a service file: the name, the content type, and the file link.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | An object with the file parameters. [Parameter description](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **productId***
[`integer`](../../../data-types.md) | Service identifier.

The identifier can be retrieved using the [catalog.product.service.list](./catalog-product-service-list-rest-v3.md) method ||
|| **fieldName***
[`string`](../../../data-types.md) | Name of the file field in uppercase.

Possible values:

- `DETAIL_PICTURE` — detail picture
- `PREVIEW_PICTURE` — preview picture
- `PROPERTY_{id}` — a property of the "File" type, where `{id}` is the property identifier

The method does not accept names in the `detailPicture` format ||
|| **fileId***
[`integer`](../../../data-types.md) | File identifier.

The identifier is returned by the [catalog.product.service.get](./catalog-product-service-get-rest-v3.md) method: in the `detailPicture` and `previewPicture` fields or in the `VALUE` key of a property of the "File" type ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.service.download`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":7551,"fieldName":"DETAIL_PICTURE","fileId":39453}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.download
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":7551,"fieldName":"DETAIL_PICTURE","fileId":39453},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.service.download
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ServiceDownloadResult = {
      file: {
        id: number
        fileName: string
        contentType: string
        src: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<ServiceDownloadResult>({
        method: 'catalog.product.service.download',
        params: {
          fields: {
            productId: 7551,
            fieldName: 'DETAIL_PICTURE',
            fileId: 39453,
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
      async function downloadServiceFile() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.service.download',
            params: {
              fields: {
                productId: 7551,
                fieldName: 'DETAIL_PICTURE',
                fileId: 39453,
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

      document.addEventListener('DOMContentLoaded', downloadServiceFile)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.service.download(
            fields={
                "productId": 7551,
                "fieldName": "DETAIL_PICTURE",
                "fileId": 39453,
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
                'catalog.product.service.download',
                [
                    'fields' => [
                        'productId' => 7551,
                        'fieldName' => 'DETAIL_PICTURE',
                        'fileId' => 39453,
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
        'catalog.product.service.download',
        {
            fields: {
                productId: 7551,
                fieldName: 'DETAIL_PICTURE',
                fileId: 39453,
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
        'catalog.product.service.download',
        [
            'fields' => [
                'productId' => 7551,
                'fieldName' => 'DETAIL_PICTURE',
                'fileId' => 39453,
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
    res, err := client.Core().Call(ctx, "catalog.product.service.download", b24.Params{
    	"fields": b24.Params{
    		"productId": 7551,
    		"fieldName": "DETAIL_PICTURE",
    		"fileId":    39453,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.service.download: %w", err)
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
            "id": 39453,
            "fileName": "montazh-detail.png",
            "contentType": "image/png",
            "src": "https://cdn-com.bitrix24.com/b1234567/iblock/0ca/0ca00fa1be74b9ef25c6759290ad85c5/montazh-detail.png"
        }
    },
    "time": {
        "start": 1792660776,
        "finish": 1792660776.659825,
        "duration": 0.6598250865936279,
        "processing": 0,
        "date_start": "2026-10-22T12:19:36+02:00",
        "date_finish": "2026-10-22T12:19:36+02:00",
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
[`object`](../../../data-types.md) | An object with the response data ||
|| **result.file**
[`object`](../../../data-types.md) | File data. [Field description](#file) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### File Object {#file}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | File identifier ||
|| **fileName**
[`string`](../../../data-types.md) | File name ||
|| **contentType**
[`string`](../../../data-types.md) | MIME type of the file, for example `image/png` ||
|| **src**
[`string`](../../../data-types.md) | File link ||
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
|| `-` | Name file field is not available | Pass `DETAIL_PICTURE`, `PREVIEW_PICTURE`, or `PROPERTY_{id}` of a property of the "File" type in uppercase in `fieldName` ||
|| `-` | Product file wrong | There is no file with this identifier in the specified service field. Check `fileId` using the [catalog.product.service.get](./catalog-product-service-get-rest-v3.md) method ||
|| `-` | Product is empty | The file was not found in the storage. Check `fileId` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | There is no service with this identifier, or `productId` is not passed. The error is also returned for the identifier of a product of another type ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions to view the catalog and read information block elements ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-get-rest-v3.md)
- [{#T}](./catalog-product-service-update-rest-v3.md)
- [{#T}](./catalog-product-service-list-rest-v3.md)
