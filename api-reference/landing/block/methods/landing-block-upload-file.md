# Upload and Attach an Image to the Block landing.block.uploadfile

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.block.uploadfile` uploads an image and attaches it to the specified block.

In response, the method returns the file identifier and a link to it in the `src` field. The image itself is not inserted into the block by the method. This means that after uploading, the file exists, but it is not yet displayed in the block's content. Typically, after `landing.block.uploadfile`, the [landing.block.updatenodes](./landing-block-update-nodes.md) method is called.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **block***
[`integer`](../../../data-types.md) | The identifier of the block to which the image should be attached.

The method accepts the identifier of any existing block.

The block identifier can be obtained using the [landing.block.getlist](./landing-block-get-list.md) method.

If the image needs to be added to a draft of a published page, use the block identifier from the draft. Typically, this is done by calling [landing.block.getlist](./landing-block-get-list.md) with the parameter `params.edit_mode = 1` ||
|| **picture***
[`string`](../../../data-types.md) \| [`string[]`](../../../data-types.md) | The image to upload.

The method only accepts images. Two formats are supported:

- Image URL,
- Array `["file_name.png", "base64_content"]`.

For the array, the file name must include the extension. In the second element of the array, only the Base64 content should be passed without the prefix `data:image/...;base64,`.

When saving, the file name may change. Cyrillic letters are transliterated by the method, and spaces and parentheses are replaced with `_`.

More about preparing Base64: [How to Upload Files](../../../files/how-to-upload-files.md) ||
|| **ext**
[`string`](../../../data-types.md) | The file extension for the URL upload, if it cannot be accurately determined from the address.

This parameter is only considered for `picture` passed as a URL. Specify the image file extension here.

If the parameter is not provided, the extension is determined automatically. For the `picture` array, the extension is taken from the file name ||
|| **params**
[`object`](../../../data-types.md) | Additional parameters for image processing [(detailed description)](#params).

If the parameter is not provided, the image is saved without resizing ||
|| **temp**
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) | If the value is converted to `true`, the file is marked as temporary.

Default is `false` ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **width**
[`integer`](../../../data-types.md) | The target width of the image in pixels.

Resizing is performed only if `height` is also provided ||
|| **height**
[`integer`](../../../data-types.md) | The target height of the image in pixels.

Resizing is performed only if `width` is also provided ||
|| **resize_type**
[`integer`](../../../data-types.md) | Resizing mode.

Possible values:
`0` — fit the image within the specified dimensions while maintaining proportions,
`1` — resize proportionally by the larger side,
`2` — adjust the image to exact dimensions, cropping if necessary.

Default is `1`. The parameter applies only to raster images ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "block": 39556,
        "picture": ["banner.png", "**base64_image_content**"],
        "params": {
          "width": 1200,
          "height": 675
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.uploadfile.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "block": 39556,
        "picture": ["banner.png", "**base64_image_content**"],
        "params": {
          "width": 1200,
          "height": 675
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.uploadfile.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.uploadfile',
    		{
    			block: 39556,
    			picture: ['banner.png', '**base64_image_content**'],
    			params: {
    				width: 1200,
    				height: 675
    			}
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.block.uploadfile',
                [
                    'block' => 39556,
                    'picture' => ['banner.png', '**base64_image_content**'],
                    'params' => [
                        'width' => 1200,
                        'height' => 675,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error uploading image: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.uploadfile',
        {
            block: 39556,
            picture: ['banner.png', '**base64_image_content**'],
            params: {
                width: 1200,
                height: 675
            }
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.block.uploadfile',
        [
            'block' => 39556,
            'picture' => ['banner.png', '**base64_image_content**'],
            'params' => [
                'width' => 1200,
                'height' => 675,
            ],
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "id": 33873,
        "src": "https://cdn-com.bitrix24.com/b13743910/landing/fda/fda6c41d4b2b4f4d672601bd48c6aff1/nature.jpg"
    },
    "time": {
        "start": 1774523909,
        "finish": 1774523910.101491,
        "duration": 1.1014909744262695,
        "processing": 1,
        "date_start": "2026-03-26T14:18:29+01:00",
        "date_finish": "2026-03-26T14:18:30+01:00",
        "operating_reset_at": 1774524509,
        "operating": 0.30655694007873535
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Data of the uploaded file [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the saved file ||
|| **src**
[`string`](../../../data-types.md) | Address of the uploaded file.

Depending on the environment, either a relative path or a full URL may be returned ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "FILE_ERROR",
    "error_description": "File upload error. The file may not be a graphic."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `block` or `picture` is missing ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found or unavailable. This error is also returned if the page cannot be determined by this identifier ||
|| `FILE_ERROR` | Failed to upload the image. This error is returned if the file could not be downloaded from the URL, read from Base64, failed the image check, SVG is not allowed on the account, or the file could not be saved ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-get-list.md)
- [{#T}](./landing-block-update-nodes.md)
- [{#T}](./landing-block-get-by-id.md)
- [{#T}](../../page/methods/landing-landing-publication.md)
- [{#T}](../../page/block-methods/landing-landing-favorite-block.md)
- [{#T}](../../page/methods/landing-landing-remove-entities.md)