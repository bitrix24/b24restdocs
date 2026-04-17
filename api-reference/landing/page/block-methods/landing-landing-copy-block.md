# Copy Block to Page `landing.landing.copyblock`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.landing.copyblock` copies a block to the specified page and returns the identifier of the created copy of the block. You can copy a block from another page or within the same page. The HTML content of the block is also copied.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of the landing. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid*** 
[`integer`](../../../data-types.md) | Identifier of the page to which the block should be copied.

The page identifier can be obtained using the method [landing.landing.getList](../methods/landing-landing-get-list.md), as well as from the results of the methods [landing.landing.add](../methods/landing-landing-add.md), [landing.landing.addByTemplate](../methods/landing-landing-add-by-template.md), and [landing.landing.copy](../methods/landing-landing-copy.md) ||
|| **block*** 
[`integer`](../../../data-types.md) | Identifier of the source block.

The block identifier can be obtained using the method [landing.block.getList](../../block/methods/landing-block-get-list.md). The block can be located on another page or on the same page `lid`.

You can only copy an existing block that is not marked as deleted. If the block is deleted, the method will return an error ||
|| **params**
[`object`](../../../data-types.md) | Additional copy parameters [(detailed description)](#params) ||
|#

### Parameter params {#params}

#| 
|| **Name**
`type` | **Description** ||
|| **AFTER_ID**
[`integer`](../../../data-types.md) | Identifier of the block on the page `lid`, after which the copy should be inserted.

If you pass the identifier of an existing block on the page `lid`, the copy will be inserted immediately after it. If the parameter is not passed, or if you pass `0` or specify the identifier of a block that does not exist on the page `lid`, the copy will be added to the end of the page ||
|| **RETURN_CONTENT**
[`string`](../../../data-types.md) | If you pass `Y`, the method will return an object with a success indicator and data of the created copy of the block [(detailed description)](#result-content). For any other value, the method will return only the identifier of the created copy of the block ||
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
        "block": 6428,
        "params": {
          "AFTER_ID": 6429,
          "RETURN_CONTENT": "Y"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.copyblock.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "block": 6428,
        "params": {
          "AFTER_ID": 6429,
          "RETURN_CONTENT": "Y"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.copyblock.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.landing.copyblock',
            {
                lid: 351,
                block: 6428,
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
                'landing.landing.copyblock',
                [
                    'lid' => 351,
                    'block' => 6428,
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
        'landing.landing.copyblock',
        {
            lid: 351,
            block: 6428,
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
        'landing.landing.copyblock',
        [
            'lid' => 351,
            'block' => 6428,
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

HTTP Status: **200**

```json
{
    "result": 28607,
    "time": {
        "start": 1773932480,
        "finish": 1773932480.485875,
        "duration": 0.48587489128112793,
        "processing": 0,
        "date_start": "2026-03-19T18:01:20+01:00",
        "date_finish": "2026-03-19T18:01:20+01:00",
        "operating_reset_at": 1773933080,
        "operating": 0
    }
}
```

If `params.RETURN_CONTENT = 'Y'` is passed, instead of the block identifier, the method will return an object with a success indicator and data of the created copy of the block.

### Example Response when `RETURN_CONTENT = 'Y'`

```json
{
    "result": {
        "result": true,
        "content": {
            "id": 28607,
            "sections": "text_image,about",
            "active": true,
            "access": "X",
            "anchor": "b28607",
            "php": false,
            "designed": false,
            "repoId": null,
            "content": "<div id=\"block28607\" data-id=\"28607\" class=\"block-wrapper block-19-2-features-with-img\">...</div>",
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
                    "name": "Text with Icon Points and Image on the Left",
                    "section": [
                        "text_image",
                        "about"
                    ]
                },
                "timestamp": 1751467642,
                "namespace": "bitrix",
                "code": "19.2.features_with_img",
                "preview": "/bitrix/blocks/bitrix/19.2.features_with_img/preview.jpg"
            },
            "dynamicParams": []
        }
    },
    "time": {
        "start": 1773933164,
        "finish": 1773933164.48191,
        "duration": 0.48190999031066895,
        "processing": 0,
        "date_start": "2026-03-19T18:12:44+01:00",
        "date_finish": "2026-03-19T18:12:44+01:00",
        "operating_reset_at": 1773933764,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) \| [`object`](../../../data-types.md) | Identifier of the created copy of the block. If `params.RETURN_CONTENT = 'Y'` is passed, the method will return an object with a success indicator and block data [(detailed description)](#result-content) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

### Object result when `RETURN_CONTENT = 'Y'` {#result-content}

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Indicator of successful copying. Returns `true` upon successful execution ||
|| **content**
[`object`](../../../data-types.md) | Data of the created copy of the block [(detailed description)](#content) ||
|#

### Object content {#content}

#| 
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the created copy of the block ||
|| **sections**
[`string`](../../../data-types.md) | Codes of the block sections from the manifest, concatenated into a single string separated by commas ||
|| **active**
[`boolean`](../../../data-types.md) | Indicator of the block's activity ||
|| **access**
[`string`](../../../data-types.md) | Access level to the block. Possible values: 
`A` — access closed to all blocks, 
`D` — access denied, 
`V` — can edit only design, 
`W` — can edit content and design without deletion, 
`X` — full access ||
|| **anchor**
[`string`](../../../data-types.md) | Local anchor of the block. The new block always receives an anchor of the form `b<ID>`, for example `b28607` ||
|| **php**
[`boolean`](../../../data-types.md) | Indicator that there is PHP code in the block's content ||
|| **designed**
[`boolean`](../../../data-types.md) | Indicator of a designed block ||
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

For REST blocks of the type `repo_<ID>`, this field returns an empty array ||
|| **assetStrings**
[`array`](../../../data-types.md) | Initialization strings for resources that need to be added to the page ||
|| **lang**
[`array`](../../../data-types.md) \| [`object`](../../../data-types.md) | Language messages of connected extensions. If messages exist, the field is returned as a key-value object. If there are no additional messages, it may return an empty array ||
|| **manifest**
[`object`](../../../data-types.md) | The complete manifest of the block. The format is described in the section [Block Manifest](../../block/manifest.md) ||
|| **dynamicParams**
[`array`](../../../data-types.md) | Parameters of the dynamic block from `SOURCE_PARAMS`. 

For regular static blocks, this field usually returns an empty array ||
|| **requiredUserAction**
[`array`](../../../data-types.md) | The field contains the same data as `manifest.requiredUserAction`. It is returned only when the user must perform additional action on the client side after copying the block ||
|#

## Error Handling

HTTP Status: **400 Bad Request**

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
|| `MISSING_PARAMS` | Required parameter `lid` or `block` is not provided ||
|| `LANDING_NOT_EXIST` | The page with identifier `lid` is not found or is not accessible to the current user ||
|| `ACCESS_DENIED` | The user does not have permission to call landing methods or does not have permission to edit the page `lid` ||
|| `BLOCK_NOT_FOUND` | The source block with identifier `block` is not found, is not accessible to the current user, is marked as deleted, or cannot be loaded from the source page ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add-block.md)
- [{#T}](./landing-landing-delete-block.md)
- [{#T}](./landing-landing-down-block.md)
- [{#T}](./landing-landing-move-block.md)
- [{#T}](./landing-landing-show-block.md)
- [{#T}](./landing-landing-up-block.md)
- [{#T}](../methods/landing-landing-publication.md)