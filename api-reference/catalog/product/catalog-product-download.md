# Download Product Files catalog.product.download

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" permission and permission to read the product in the information block

The `catalog.product.download` method downloads product files from the trade catalog using the provided parameters.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Parameters of the file to download. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **fileId***
[`integer`](../../data-types.md) | Identifier of the file.

The identifier is returned by the [catalog.product.get](./catalog-product-get.md) and [catalog.product.list](./catalog-product-list.md) methods in the `detailPicture` and `previewPicture` fields or in the `value` of a "File" type property ||
|| **productId***
[`catalog_product.id`](../data-types.md#catalog_product) | Identifier of the product.

You can retrieve product identifiers using the [catalog.product.list](./catalog-product-list.md) method ||
|| **fieldName***
[`string`](../../data-types.md) | Product field where the file is stored:

- `detailPicture` — detailed image
- `previewPicture` — preview image
- `propertyN` — a "File" type property, where `N` is the property identifier. Instead of the identifier, you can specify the symbolic code of the property in camelCase, for example `propertyMorePhoto` for the `MORE_PHOTO` code

In [catalog.product.get](./catalog-product-get.md) and [catalog.product.list](./catalog-product-list.md) responses, the `fieldName` value is already included in the file's `url` and `urlMachine`. Pass it in the same form. The method does not accept uppercase names, such as `DETAIL_PICTURE` or `PROPERTY_N`.

You can retrieve property identifiers using the [catalog.productProperty.list](../product-property/catalog-product-property-list.md) method ||
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const auth = $b24.auth.getAuthData()
      if (!auth) {
        throw new Error('No auth data')
      }

      // The method returns a binary file, so the request is sent directly
      const response = await fetch(`${$b24.getTargetOriginWithPath()}/catalog.product.download`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          fields: {
            fileId: 6439,
            productId: 1243,
            fieldName: 'detailPicture',
          },
          auth: auth.access_token,
        }),
      })

      if (!response.ok) {
        const error = await response.json()
        throw new Error(error.error_description)
      }

      const blob: Blob = await response.blob()
      console.info('File size:', blob.size, 'type:', blob.type)
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function downloadProductFile() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()
          const auth = $b24.auth.getAuthData()
          if (!auth) {
            throw new Error('No auth data')
          }

          // The method returns a binary file, so the request is sent directly
          const response = await fetch(`${$b24.getTargetOriginWithPath()}/catalog.product.download`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
              fields: {
                fileId: 6439,
                productId: 1243,
                fieldName: 'detailPicture',
              },
              auth: auth.access_token,
            }),
          })

          if (!response.ok) {
            const error = await response.json()
            throw new Error(error.error_description)
          }

          // Save the file in the browser
          const blob = await response.blob()
          const link = document.createElement('a')
          link.href = URL.createObjectURL(blob)
          link.download = 'product-picture.png'
          link.click()
          URL.revokeObjectURL(link.href)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', downloadProductFile)
    </script>
    ```

- Python

    ```python
    import json
    from urllib.error import HTTPError
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

    try:
        with urlopen(request) as response, open("product-picture.png", "wb") as output:
            output.write(response.read())
    except HTTPError as error:
        print("Bitrix API error:", json.loads(error.read()).get("error_description"))
    ```

- PHP

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

- BX24.js

    ```js
    // The method returns a binary file, so the request is sent directly
    const auth = BX24.getAuth();

    fetch(`https://${auth.domain}/rest/catalog.product.download`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
            fields: {
                fileId: 6439,
                productId: 1243,
                fieldName: 'detailPicture'
            },
            auth: auth.access_token
        })
    })
        .then(response => response.ok
            ? response.blob()
            : response.json().then(error => { throw new Error(error.error_description); }))
        .then(blob => {
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = 'product-picture.png';
            link.click();
            URL.revokeObjectURL(link.href);
        })
        .catch(error => console.error(error));
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    // CRest::call() decodes the response as JSON, so the binary file is requested directly.
    // C_REST_WEB_HOOK_URL is set in settings.php
    $request = curl_init(C_REST_WEB_HOOK_URL . 'catalog.product.download');
    curl_setopt_array($request, [
        CURLOPT_POST => true,
        CURLOPT_POSTFIELDS => http_build_query([
            'fields' => [
                'fileId' => 6439,
                'productId' => 1243,
                'fieldName' => 'detailPicture',
            ],
        ]),
        CURLOPT_RETURNTRANSFER => true,
    ]);

    $file = curl_exec($request);
    $status = curl_getinfo($request, CURLINFO_HTTP_CODE);
    curl_close($request);

    if ($status === 200) {
        file_put_contents('product-picture.png', $file);
    } else {
        echo '<PRE>';
        print_r(json_decode($file, true));
        echo '</PRE>';
    }
    ```

- Go

    ```go
    // The method returns a binary file, so the request is sent directly
    url := "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.download"
    body, err := json.Marshal(map[string]any{
    	"fields": map[string]any{
    		"fileId":    6439,
    		"productId": 1243,
    		"fieldName": "detailPicture",
    	},
    })
    if err != nil {
    	return err
    }

    resp, err := http.Post(url, "application/json", bytes.NewReader(body))
    if err != nil {
    	return fmt.Errorf("catalog.product.download: %w", err)
    }
    defer resp.Body.Close()

    if resp.StatusCode != http.StatusOK {
    	var apiErr struct {
    		Error            string `json:"error"`
    		ErrorDescription string `json:"error_description"`
    	}
    	_ = json.NewDecoder(resp.Body).Decode(&apiErr)
    	return fmt.Errorf("catalog.product.download: %s", apiErr.ErrorDescription)
    }

    file, err := os.Create("product-picture.png")
    if err != nil {
    	return err
    }
    defer file.Close()
    if _, err := io.Copy(file, resp.Body); err != nil {
    	return fmt.Errorf("save file: %w", err)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The response contains the file content with the `Content-Type` and `Content-Disposition` headers, for example `Content-Type: image/png` and `Content-Disposition: attachment; filename="logo.png"`. The method checks that `fileId` belongs to the specified product and `fieldName`. Save the response as a file: a successful call has no JSON `result` field.

### Returned Data

The file content is returned, not a JSON `result` object. Retrieve the file ID and URL from `detailPicture`, `previewPicture`, or a file property in the [catalog.product.get](./catalog-product-get.md) response.

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Product file wrong"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Cause** ||
|| `400` | `200040300010` | `Access Denied` | The user has none of the "View product catalog", "View catalog products", or "Edit selling price" permissions ||
|| `400` | `200040300000` | `Iblock Not Found` | The product's information block was not found ||
|| `400` | `200040300040` | `Access Denied` | No permission to read the product in the information block ||
|| `400` | Empty value | `product does not exist.` | No product exists with the specified `productId` ||
|| `400` | `100` | `Could not find value for parameter {fields}` | The `fields` parameter was not provided ||
|| `400` | `0` | `Required fields: fieldName, fileId, productId` | Required fields were not provided. The description lists only the missing fields ||
|| `400` | `0` | `Name file field is not available` | `fieldName` is not a "File" type field or property, or it was passed in uppercase ||
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
