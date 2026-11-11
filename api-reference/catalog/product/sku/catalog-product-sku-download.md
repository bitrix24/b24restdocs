# Download Parent Product Files catalog.product.sku.download

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission and the permission to read the catalog information block element

The `catalog.product.sku.download` method returns a parent product file: the preview image, the detailed image, or a file from a "File" type property.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Parameters of the file to download ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fileId***
[`integer`](../../../data-types.md) | File identifier.

You can retrieve the identifier using the [catalog.product.sku.get](./catalog-product-sku-get.md) method: it is the `id` field in the `previewPicture` or `detailPicture` object or in the file property value ||
|| **productId***
[`catalog_product_sku.id`](../../data-types.md#catalog_product_sku) | Identifier of the parent product.

You can retrieve parent product identifiers using the [catalog.product.sku.list](./catalog-product-sku-list.md) method ||
|| **fieldName***
[`string`](../../../data-types.md) | Name of the field that stores the file. Pass the name in camelCase:
- `previewPicture` — preview image
- `detailPicture` — detailed image
- `propertyN` — a "File" type property, where `N` is the property identifier, for example `property309`

The method rejects uppercase names, such as `DETAIL_PICTURE` or `PROPERTY_309`, with the `Name file field is not available` error.

You can retrieve property identifiers using the [catalog.product.sku.getFieldsByFilter](./catalog-product-sku-get-fields-by-filter.md) method: for file properties, `propertyType` is `F` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fileId":40051,"productId":7679,"fieldName":"detailPicture"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.download
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fileId":40051,"productId":7679,"fieldName":"detailPicture"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.sku.download
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
        method: 'catalog.product.sku.download',
        params: {
          fields: {
            fileId: 40051,
            productId: 7679,
            fieldName: 'detailPicture',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result)
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
      async function downloadProductSku() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.sku.download',
            params: {
              fields: {
                fileId: 40051,
                productId: 7679,
                fieldName: 'detailPicture',
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
          console.info(result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', downloadProductSku)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.sku.download(
            fields={
                "fileId": 40051,
                "productId": 7679,
                "fieldName": "detailPicture",
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
                'catalog.product.sku.download',
                [
                    'fields' => [
                        'fileId'     => 40051,
                        'productId'  => 7679,
                        'fieldName'  => 'detailPicture',
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

    ```js
    BX24.callMethod(
        'catalog.product.sku.download',
        {
            fields: {
                fileId: 40051,
                productId: 7679,
                fieldName: 'detailPicture',
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
        'catalog.product.sku.download',
        [
            'fields' => [
                'fileId' => 40051,
                'productId' => 7679,
                'fieldName' => 'detailPicture'
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
    res, err := client.Core().Call(ctx, "catalog.product.sku.download", b24.Params{
    	"fields": b24.Params{
    		"fileId":    40051,
    		"productId": 7679,
    		"fieldName": "detailPicture",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.sku.download: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The method returns the file itself, not JSON. The `Content-Type` header matches the file type, for example `image/png`, and the file name is passed in the `Content-Disposition` header.

### Returned Data

The file content. Save the response body to a file with the name from the `Content-Disposition` header.

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Product file wrong"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user does not have a permission to view the catalog ||
|| `400` | `200040300040` | Access Denied | The user does not have the permission to read the catalog information block element ||
|| `400` | Empty value | sku does not exist. | No product exists with the specified `productId` ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not provided ||
|| `400` | `0` | Required fields: fileId | The `fileId` field is not provided in `fields`. The same error with the field name is returned for other required fields: `Required fields: productId`, `Required fields: fieldName` ||
|| `400` | `0` | Name file field is not available | `fieldName` contains a field that does not store files, or an uppercase name ||
|| `400` | `0` | Product file wrong | The `fieldName` field of this product has no file with the specified `fileId` ||
|| `400` | `0` | Product is empty | No file exists with the specified `fileId` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-sku-add.md)
- [{#T}](./catalog-product-sku-update.md)
- [{#T}](./catalog-product-sku-get.md)
- [{#T}](./catalog-product-sku-list.md)
- [{#T}](./catalog-product-sku-delete.md)
- [{#T}](./catalog-product-sku-get-fields-by-filter.md)