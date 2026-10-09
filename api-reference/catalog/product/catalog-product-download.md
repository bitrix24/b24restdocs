# Download Product Files catalog.product.download

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the product catalog

The `catalog.product.download` method downloads product files from the trade catalog using the provided parameters.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields*** 
 [`object`](../../data-types.md)| Field values for downloading product files ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fileId*** 
 [`integer`](../../data-types.md)| Identifier of the registered file.

To obtain file identifiers for products, use [catalog.product.get](./catalog-product-get.md) or [catalog.product.list](./catalog-product-list.md)
 ||
|| **productId*** 
 [`catalog_product.id`](../data-types.md#catalog_product)| Identifier of the product.

To obtain product identifiers, use [catalog.product.list](./catalog-product-list.md)
 ||
|| **fieldName*** 
[`string`](../../data-types.md) | Name of the field (property or field of the information block element) where the file is stored.

- `detailPicture` — detailed image
- `previewPicture` — preview image
- `propertyN` — file property, where `N` is the property ID or code

In [catalog.product.get](./catalog-product-get.md) and [catalog.product.list](./catalog-product-list.md) responses, the `fieldName` value is already included in the file's `url` and `urlMachine`. Pass it in the same form. Internally, the method converts field names to `DETAIL_PICTURE`, `PREVIEW_PICTURE`, and `PROPERTY_N`

To obtain existing identifiers or property codes for products, use [catalog.productProperty.list](../product-property/catalog-product-property-list.md)
 ||
|#

## Code Examples

The examples use direct HTTP requests to save the binary response.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl --fail -X POST \
    -H "Content-Type: application/json" \
    -d '{"fields":{"fileId":6439,"productId":1243,"fieldName":"detailPicture"}}' \
    -o product-picture.png \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.download
    ```

- cURL (OAuth)

    ```bash
    curl --fail -X POST \
    -H "Content-Type: application/json" \
    -d '{"fields":{"fileId":6439,"productId":1243,"fieldName":"detailPicture"},"auth":"**put_access_token_here**"}' \
    -o product-picture.png \
    https://**put_your_bitrix24_address**/rest/catalog.product.download
    ```

- Python (Webhook)

    ```python
    import json
    from urllib.request import Request, urlopen

    url = "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.download"
    payload = {
        "fields": {
            "fileId": 6439,
            "productId": 1243,
            "fieldName": "detailPicture",
        }
    }
    request = Request(
        url,
        data=json.dumps(payload).encode("utf-8"),
        headers={"Content-Type": "application/json"},
        method="POST",
    )

    with urlopen(request) as response, open("product-picture.png", "wb") as output:
        output.write(response.read())
    ```

- PHP (Webhook)

    ```php
    <?php
    $url = 'https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.download';
    $payload = json_encode([
        'fields' => [
            'fileId' => 6439,
            'productId' => 1243,
            'fieldName' => 'detailPicture',
        ],
    ], JSON_THROW_ON_ERROR);

    $request = curl_init($url);
    curl_setopt_array($request, [
        CURLOPT_POST => true,
        CURLOPT_POSTFIELDS => $payload,
        CURLOPT_HTTPHEADER => ['Content-Type: application/json'],
        CURLOPT_RETURNTRANSFER => true,
    ]);

    $file = curl_exec($request);
    $status = curl_getinfo($request, CURLINFO_HTTP_CODE);
    $error = curl_error($request);
    curl_close($request);
    if ($file === false || $status !== 200) {
        throw new RuntimeException('Failed to download product file: HTTP ' . $status . ' ' . $error);
    }
    file_put_contents('product-picture.png', $file);
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The response contains the file content. The method checks that `fileId` belongs to the specified product and `fieldName`. Save the response as a file: a successful call has no JSON `result` field.

### Returned Data

The file content is returned, not a JSON `result` object. Retrieve the file ID and URL from `detailPicture`, `previewPicture`, or a file property in the [catalog.product.get](./catalog-product-get.md) response.

## Error Handling

HTTP status: **400**

```json
{	
   "error":0,
   "error_description":"Required fields: fileId"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Cause** ||
|| `400` | `200040300010` | `Access Denied` | No permission to view the product catalog ||
|| `400` | Empty code | `product does not exist.` | No product exists with the specified `productId` ||
|| `400` | `0` | `Required fields: fieldName, fileId, productId` | Required fields were not provided; the description lists the missing fields ||
|| `400` | `0` | `Name file field is not available` | `fieldName` is not a valid product file field ||
|| `400` | `0` | `Product file wrong` | `fileId` does not belong to the specified product and field ||
|| `400` | `0` | `Product is empty` | The file record was not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-product-add.md)
- [{#T}](./catalog-product-update.md)
- [{#T}](./catalog-product-get.md)
- [{#T}](./catalog-product-list.md)
- [{#T}](./catalog-product-delete.md)
- [{#T}](./catalog-product-get-fields-by-filter.md)
