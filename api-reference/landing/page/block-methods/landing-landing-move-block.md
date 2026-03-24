# Move Block to Page `landing.landing.moveblock`

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for the site

The method `landing.landing.moveblock` moves a block to the specified page and returns the identifier of the moved block. The block can be moved within the same page or to a different page.

If the pages are already published, changes will be visible to visitors after the "Publish changes" command in the interface or after calling the method [landing.landing.publication](../methods/landing-landing-publication.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Identifier of the page to which the block should be moved.

The page identifier can be obtained using the method [landing.landing.getList](../methods/landing-landing-get-list.md), as well as from the results of the methods [landing.landing.add](../methods/landing-landing-add.md), [landing.landing.addByTemplate](../methods/landing-landing-add-by-template.md), and [landing.landing.copy](../methods/landing-landing-copy.md) ||
|| **block***
[`integer`](../../../data-types.md) | Identifier of the block.

The block identifier should be obtained using the method [landing.block.getList](../../block/methods/landing-block-get-list.md) with the parameter `params.edit_mode = 1`. If the block is being moved from another page, request blocks from the source page.

The block can be on another page or on the same page `lid`. Only an existing block that is not marked as deleted can be moved. ||
|| **params**
[`object`](../../../data-types.md) | Additional parameters for the move [(detailed description)](#params) ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **AFTER_ID**
[`integer`](../../../data-types.md) | Identifier of the block on the page `lid`, after which the moved block should be placed.

If the parameter is not provided, is set to `0`, or points to a block that does not exist on the page `lid`, the block will be moved to the end of the page. ||
|| **RETURN_CONTENT**
[`string`](../../../data-types.md) | If set to `Y`, the method will return an object with a success flag and data of the moved block [(detailed description)](#result-content). For any other value, the method will return only the identifier of the moved block. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "block": 26723,
        "params": {
          "AFTER_ID": 6429,
          "RETURN_CONTENT": "Y"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.moveblock.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "block": 26723,
        "params": {
          "AFTER_ID": 6429,
          "RETURN_CONTENT": "Y"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.moveblock.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.landing.moveblock',
            {
                lid: 351,
                block: 26723,
                params: {
                    AFTER_ID: 6429,
                    RETURN_CONTENT: 'Y'
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
                'landing.landing.moveblock',
                [
                    'lid' => 351,
                    'block' => 26723,
                    'params' => [
                        'AFTER_ID' => 6429,
                        'RETURN_CONTENT' => 'Y',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.moveblock',
        {
            lid: 351,
            block: 26723,
            params: {
                AFTER_ID: 6429,
                RETURN_CONTENT: 'Y'
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
        'landing.landing.moveblock',
        [
            'lid' => 351,
            'block' => 26723,
            'params' => [
                'AFTER_ID' => 6429,
                'RETURN_CONTENT' => 'Y',
            ],
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true,
        "content": {
            "id": 26723,
            "sections": "text_image,recommended,widgets_image",
            "active": true,
            "access": "X",
            "anchor": "b383315",
            "php": false,
            "designed": true,
            "repoId": null,
            "content": "<div id=\"block26723\" data-id=\"26723\" class=\"block-wrapper block-31-3-two-cols-text-img-fix\">...</div>",
            "content_ext": "",
            "css": [],
            "js": [
                "/bitrix/js/pull/protobuf/protobuf.js?1592315491274055",
                "/bitrix/js/pull/protobuf/model.min.js?159231549114190",
                "/bitrix/js/main/core/core_promise.min.js?17647596972494",
                "/bitrix/js/rest/client/rest.client.min.js?16015491189240",
                "/bitrix/js/pull/client/pull.client.min.js?174471771449849"
            ],
            "assetStrings": [],
            "lang": [],
            "manifest": {
                "block": {
                    "name": "Text with Image on the Right",
                    "type": [
                        "page",
                        "store",
                        "smn",
                        "knowledge",
                        "group",
                        "mainpage"
                    ],
                    "section": [
                        "text_image",
                        "recommended",
                        "widgets_image"
                    ]
                },
                "timestamp": 1751467642,
                "namespace": "bitrix",
                "code": "31.3.two_cols_text_img_fix",
                "preview": "/bitrix/blocks/bitrix/31.3.two_cols_text_img_fix/preview.jpg"
            },
            "dynamicParams": []
        }
    },
    "time": {
        "start": 1774062200,
        "finish": 1774062200.463154,
        "duration": 0.4631540775299072,
        "processing": 0,
        "date_start": "2026-03-21T06:03:20+02:00",
        "date_finish": "2026-03-21T06:03:20+02:00",
        "operating_reset_at": 1774062800,
        "operating": 0
    }
}
```

### Example Response without `RETURN_CONTENT`

```json
{
    "result": 26723,
    "time": {
        "start": 1774062265,
        "finish": 1774062265.312441,
        "duration": 0.3124408721923828,
        "processing": 0,
        "date_start": "2026-03-21T06:04:25+02:00",
        "date_finish": "2026-03-21T06:04:25+02:00",
        "operating_reset_at": 1774062865,
        "operating": 0
    }
}
```
### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) \| [`object`](../../../data-types.md) | Identifier of the moved block. If `params.RETURN_CONTENT = 'Y'` is provided, the method will return an object with a success flag and block data [(detailed description)](#result-content) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Object result when `RETURN_CONTENT = 'Y'` {#result-content}

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Success flag for the move. Returns `true` on successful execution. ||
|| **content**
[`object`](../../../data-types.md) | Data of the moved block [(detailed description)](#content) ||
|#

### Object content {#content}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the moved block ||
|| **sections**
[`string`](../../../data-types.md) | Codes of the block sections from the manifest, concatenated into a single string separated by commas ||
|| **active**
[`boolean`](../../../data-types.md) | Active flag of the block ||
|| **access**
[`string`](../../../data-types.md) | Access level to the block. Possible values:
`A` — the current user does not have permission to edit the page,
`D` — access denied,
`V` — can only edit the design, which is insufficient for moving the block,
`W` — can edit content and design without deletion,
`X` — full access ||
|| **anchor**
[`string`](../../../data-types.md) | Local anchor of the block ||
|| **php**
[`boolean`](../../../data-types.md) | Indicates whether there is PHP code in the block's content ||
|| **designed**
[`boolean`](../../../data-types.md) | Indicates whether the block is a designed block ||
|| **repoId**
[`integer`](../../../data-types.md) | Identifier of the REST block from the repository or `null` if the block is not linked to a REST repository ||
|| **content**
[`string`](../../../data-types.md) | Prepared HTML code of the block ||
|| **content_ext**
[`string`](../../../data-types.md) | Additional HTML code of connected extensions ||
|| **css**
[`array`](../../../data-types.md) | List of CSS resources for the block. If there are no separate CSS connections for the block, an empty array is returned ||
|| **js**
[`array`](../../../data-types.md) | List of JS resources for the block and related client libraries that need to be included for its operation.

For REST blocks of the type `repo_<ID>`, this field returns an empty array. ||
|| **assetStrings**
[`array`](../../../data-types.md) | Initialization strings for resources that need to be added to the page ||
|| **lang**
[`array`](../../../data-types.md) \| [`object`](../../../data-types.md) | Language messages of connected extensions. If messages exist, this field is returned as a key-value object. If there are no additional messages, it may return an empty array ||
|| **manifest**
[`object`](../../../data-types.md) | The complete manifest of the block. The format is described in the section [Block Manifest](../../block/manifest.md) ||
|| **dynamicParams**
[`array`](../../../data-types.md) | Parameters of the dynamic block from `SOURCE_PARAMS`.

For regular static blocks, this field usually returns an empty array. ||
|| **requiredUserAction**
[`array`](../../../data-types.md) | This field contains the same data as `manifest.requiredUserAction`. It is returned when the user must perform additional actions on the client side after moving the block. ||
|#

## Error Handling

HTTP status: **400 Bad Request**

```json
{
    "error": "BLOCK_NOT_FOUND",
    "error_description": "Block not found in the landing"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid` or `block` is missing ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user ||
|| `ACCESS_DENIED` | Insufficient permissions to call the method ||
|| `BLOCK_NOT_FOUND` | The source block with identifier `block` not found, not accessible to the current user, marked as deleted, provided from the published version of the page, or cannot be loaded from the source page ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add-block.md)
- [{#T}](./landing-landing-copy-block.md)
- [{#T}](./landing-landing-delete-block.md)
- [{#T}](./landing-landing-down-block.md)
- [{#T}](./landing-landing-hide-block.md)
- [{#T}](./landing-landing-mark-deleted-block.md)
- [{#T}](./landing-landing-mark-undeleted-block.md)
- [{#T}](./landing-landing-show-block.md)
- [{#T}](./landing-landing-up-block.md)
- [{#T}](../methods/landing-landing-publication.md)