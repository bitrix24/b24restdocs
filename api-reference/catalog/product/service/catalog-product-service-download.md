# Download Service Files catalog.product.service.download

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with one of the "View product catalog", "View catalog products", or "Edit selling price" access permissions and the permission to read elements of the catalog information block

The `catalog.product.service.download` method downloads a service file.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Service file parameters [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fileId***
[`integer`](../../../data-types.md) | File identifier. You can retrieve it using the [catalog.product.service.get](./catalog-product-service-get.md) or [catalog.product.service.list](./catalog-product-service-list.md) method: the `id` field in `detailPicture`, `previewPicture`, or the file property value ||
|| **productId***
[`catalog_product_service.id`](../../data-types.md#catalog_product_service) | Identifier of the service. You can retrieve the identifier using the [catalog.product.service.list](./catalog-product-service-list.md) method ||
|| **fieldName***
[`string`](../../../data-types.md) | Name of the field (property or field of the information block element) where the file is stored. Possible values:
- `detailPicture` — detailed image
- `previewPicture` — preview image
- `propertyN` — file property, where `N` is the property identifier or symbolic code, for example, `property258` or `propertyMorePhoto`

You can retrieve service property identifiers and symbolic codes using [catalog.productProperty.list](../../product-property/catalog-product-property-list.md). Only file-type properties can be downloaded
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fileId":40055,"productId":7623,"fieldName":"previewPicture"}}' \
    -o service-picture.png \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.download
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fileId":40055,"productId":7623,"fieldName":"previewPicture"},"auth":"**put_access_token_here**"}' \
    -o service-picture.png \
    https://**put_your_bitrix24_address**/rest/catalog.product.service.download
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'catalog.product.service.download',
        params: {
          fields: {
            fileId: 40055,
            productId: 7623,
            fieldName: 'previewPicture',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('File downloaded successfully:', result)
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
      async function downloadServiceFile() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.service.download',
            params: {
              fields: {
                fileId: 40055,
                productId: 7623,
                fieldName: 'previewPicture',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('File downloaded successfully:', result)
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
                "fileId": 40055,
                "productId": 7623,
                "fieldName": "previewPicture",
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.product.service.download',
                [
                    'fields' => [
                        'fileId'    => 40055,
                        'productId' => 7623,
                        'fieldName' => 'previewPicture',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error downloading product service: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.service.download',
        {
            fields: {
                fileId: 40055,
                productId: 7623,
                fieldName: 'previewPicture',
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.product.service.download',
        [
            'fields' => [
                'fileId' => 40055,
                'productId' => 7623,
                'fieldName' => 'previewPicture'
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
    res, err := client.Core().Call(ctx, "catalog.product.service.download", b24.Params{
    	"fields": b24.Params{
    		"fileId":    40055,
    		"productId": 7623,
    		"fieldName": "previewPicture",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.service.download: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The method returns the file itself, not JSON. The `Content-Type` header matches the file, for example `image/png`, and the file name is passed in the `Content-Disposition` header, for example `attachment; filename="ac.png"`.

### Returned Data

The file content. Save the response body to a file with the name from the `Content-Disposition` header. You can retrieve the file identifier from `detailPicture`, `previewPicture`, or a file property in the [catalog.product.service.get](./catalog-product-service-get.md) response.

## Error Handling

HTTP status: **400**

```json
{
    "error":"0",
    "error_description":"Required fields: fileId"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has none of the "View product catalog", "View catalog products", or "Edit selling price" access permissions ||
|| `400` | `200040300040` | Access Denied | The user does not have the permission to read the catalog information block element ||
|| `400` | Empty value | service does not exist. | There is no service with the specified `productId` ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not provided ||
|| `400` | `0` | Required fields: fileId | A required field is not provided. The text lists the missing fields ||
|| `400` | `0` | Name file field is not available | `fieldName` contains a field that does not store files ||
|| `400` | `0` | Product file wrong | The file with the specified `fileId` is not in the `fieldName` field of the service ||
|| `400` | `0` | Product is empty | The file was not found in the storage ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-add.md)
- [{#T}](./catalog-product-service-update.md)
- [{#T}](./catalog-product-service-get.md)
- [{#T}](./catalog-product-service-list.md)
- [{#T}](./catalog-product-service-delete.md)
- [{#T}](./catalog-product-service-get-fields-by-filter.md)
