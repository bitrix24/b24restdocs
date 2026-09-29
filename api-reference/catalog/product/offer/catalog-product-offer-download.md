# Download Product Variation Files catalog.product.offer.download

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `catalog.product.offer.download` downloads a product variation file from an image field or a file property.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Product variation file parameters [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fileId***
[`integer`](../../../data-types.md) | Identifier of the registered file.

Use the `id` field of the file object returned by [catalog.product.offer.get](./catalog-product-offer-get.md) or [catalog.product.offer.list](./catalog-product-offer-list.md)
||
|| **productId***
[`catalog_product_offer.id`](../../data-types.md#catalog_product_offer) | Identifier of the product variation.

To obtain the identifiers of product variations, you need to use [catalog.product.offer.list](./catalog-product-offer-list.md)
||
|| **fieldName***
[`string`](../../../data-types.md) | Name of the field where the file is stored. Pass the name in camelCase, in the same format returned by [catalog.product.offer.get](./catalog-product-offer-get.md) and [catalog.product.offer.list](./catalog-product-offer-list.md).

Possible values:

- `detailPicture` — detailed image; the field is available in the old product form
- `previewPicture` — preview image; the field is available in the old product form
- `propertyN` — file property, where `N` is the property identifier or symbolic code, for example `property258` or `propertyMorePhoto`

You can obtain product variation property identifiers and symbolic codes using [catalog.productProperty.list](../../product-property/catalog-product-property-list.md). Only file-type properties can be downloaded
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The method returns the file contents, not JSON. Send a direct HTTP request and save the body of a successful response as a file.

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{"fields":{"fileId":6538,"productId":1286,"fieldName":"detailPicture"}}' \
    --output offer-file \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.download
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{"fields":{"fileId":6538,"productId":1286,"fieldName":"detailPicture"},"auth":"**put_access_token_here**"}' \
    --output offer-file \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.download
    ```

- JS (TS)

    ```ts
    import { writeFile } from 'node:fs/promises'

    const response = await fetch(
      'https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.download',
      {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          fields: {
            fileId: 6538,
            productId: 1286,
            fieldName: 'detailPicture',
          },
        }),
      },
    )

    if (!response.ok) {
      throw new Error(await response.text())
    }

    const file = new Uint8Array(await response.arrayBuffer())
    await writeFile('offer-file', file)
    ```

- JS (UMD)

    ```html
    <script>
      async function downloadOfferFile() {
        const response = await fetch(
          'https://**put_your_bitrix24_address**/rest/catalog.product.offer.download',
          {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify({
              fields: {
                fileId: 6538,
                productId: 1286,
                fieldName: 'detailPicture',
              },
              auth: '**put_access_token_here**',
            }),
          },
        )

        if (!response.ok) {
          throw new Error(await response.text())
        }

        const blob = await response.blob()
        const url = URL.createObjectURL(blob)
        const link = document.createElement('a')
        link.href = url
        link.download = 'offer-file'
        document.body.appendChild(link)
        link.click()
        link.remove()
        URL.revokeObjectURL(url)
      }

      document.addEventListener('DOMContentLoaded', downloadOfferFile)
    </script>
    ```

- Python

    ```python
    import requests

    response = requests.post(
        "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.download",
        json={
            "fields": {
                "fileId": 6538,
                "productId": 1286,
                "fieldName": "detailPicture",
            }
        },
    )
    response.raise_for_status()

    with open("offer-file", "wb") as file:
        file.write(response.content)
    ```

- PHP

    ```php
    $curl = curl_init('https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.download');
    curl_setopt_array($curl, [
        CURLOPT_POST => true,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_HTTPHEADER => ['Content-Type: application/json'],
        CURLOPT_POSTFIELDS => json_encode([
            'fields' => [
                'fileId' => 6538,
                'productId' => 1286,
                'fieldName' => 'detailPicture',
            ],
        ]),
    ]);

    $response = curl_exec($curl);
    $httpCode = curl_getinfo($curl, CURLINFO_HTTP_CODE);
    curl_close($curl);

    if ($httpCode === 200) {
        file_put_contents('offer-file', $response);
    } else {
        echo $response;
    }
    ```

- BX24.js

    ```js
    const auth = BX24.getAuth()
    const url = new URL(`https://${auth.domain}/rest/catalog.product.offer.download`)
    url.searchParams.set('auth', auth.access_token)
    url.searchParams.set('fields[fileId]', 6538)
    url.searchParams.set('fields[productId]', 1286)
    url.searchParams.set('fields[fieldName]', 'detailPicture')

    fetch(url)
      .then(async response => {
        if (!response.ok) {
          throw new Error(await response.text())
        }
        return response.blob()
      })
      .then(blob => {
        const objectUrl = URL.createObjectURL(blob)
        const link = document.createElement('a')
        link.href = objectUrl
        link.download = 'offer-file'
        document.body.appendChild(link)
        link.click()
        link.remove()
        URL.revokeObjectURL(objectUrl)
      })
      .catch(error => console.error(error))
    ```

- Go

    ```go
    payload, err := json.Marshal(map[string]any{
        "fields": map[string]any{
            "fileId": 6538, "productId": 1286, "fieldName": "detailPicture",
        },
    })
    if err != nil {
        return err
    }

    url := "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.download"
    response, err := http.Post(url, "application/json", bytes.NewReader(payload))
    if err != nil {
        return err
    }
    defer response.Body.Close()

    body, err := io.ReadAll(response.Body)
    if err != nil {
        return err
    }
    if response.StatusCode != http.StatusOK {
        return fmt.Errorf("download failed: %s", body)
    }

    return os.WriteFile("offer-file", body, 0o644)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The response body contains the file contents. This is a binary response, not JSON: the `Content-Type` header depends on the file format, and `Content-Disposition: attachment` contains the download file name.

### Returned Data

A file with the `fileId` identifier from the `fieldName` field of the `productId` variation.

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: fileId"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **error_description** | **Description** ||
|| `200040300010` | `Access denied` | Insufficient permissions to read the product catalog
||
|| Empty value | `offer does not exist.` | The product variation with the specified `productId` does not exist
||
|| `0` | `Name file field is not available` | The `fieldName` field is unavailable for download: the name is incorrect, the property does not exist, or it is not a file-type property
||
|| `0` | `Product file wrong` | The `fileId` file does not belong to the specified variation field
||
|| `0` | `Product is empty` | The file with the specified `fileId` was not found in file storage
||
|| `0` | `Required fields: fieldName, fileId, productId` | One or more required parameters are missing. The message lists the missing parameters
||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-add.md)
- [{#T}](./catalog-product-offer-update.md)
- [{#T}](./catalog-product-offer-get.md)
- [{#T}](./catalog-product-offer-list.md)
- [{#T}](./catalog-product-offer-delete.md)
- [{#T}](./catalog-product-offer-get-fields-by-filter.md)
