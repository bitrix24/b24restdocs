# Save to the block list landing.landing.favoriteBlock

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for the site

The method `landing.landing.favoriteBlock` creates a copy of a page block and saves it to the block list as a template. Upon successful execution, the method returns the identifier of the new block copy.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the [landing.landing.getList](../methods/landing-landing-get-list.md) method, as well as from the results of the [landing.landing.add](../methods/landing-landing-add.md), [landing.landing.addByTemplate](../methods/landing-landing-add-by-template.md), and [landing.landing.copy](../methods/landing-landing-copy.md) methods ||
|| **block***
[`integer`](../../../data-types.md) | Block identifier.

The block identifier can be obtained using the [landing.block.getList](../../block/methods/landing-block-get-list.md) method with the parameter `params.edit_mode = 1` ||
|| **meta**
[`object`](../../../data-types.md) | Parameters of the saved block [(detailed description)](#meta).

If the parameter is not provided, the method saves a copy of the block without additional overrides ||
|#

### Parameter meta {#meta}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Name of the saved block copy in the block list.

This is the user-defined name of the saved copy. It does not replace the standard name of the block. If the parameter is not provided, the original block name is used ||
|| **section**
[`array`](../../../data-types.md) \| [`string`](../../../data-types.md) | Section or list of sections where the saved block will be displayed. If the parameter is not provided, the block will be shown in the same sections as the original block.

You can use section codes from the [landing.block.getrepository](../../block/methods/landing-block-get-repository.md) method. If you provide a code that does not exist in the standard repository, a separate section will be created for it ||
|| **preview**
[`integer`](../../../data-types.md) | Identifier of the preview image file.

Provide the file ID of the image that has been pre-uploaded using the [landing.block.uploadfile](../../block/methods/landing-block-upload-file.md) method for the original block. The method saves this ID as the preview for the saved block copy.

If the file is already linked to the original block as a user preview, this link will only change after the method successfully completes. If the method fails, the link to the original block will not change. If the parameter is not provided or equals `0`, the user preview is not saved ||
|| **tpl_code**
[`string`](../../../data-types.md) | Template code of the page to which the saved block should be linked.

Providing this parameter is equivalent to checking the "Link to current style" checkbox in the editor. The value can be taken from the `TPL_CODE` field of the page, for example from the response of the [landing.landing.getList](../methods/landing-landing-get-list.md) method.

This parameter is saved along with the metadata of the saved block and is used by the editor when working with such blocks. If the parameter is not provided, no special link to the template code is saved ||
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
        "meta": {
          "name": "Block with Benefits",
          "section": ["text", "features"],
          "preview": 918273,
          "tpl_code": "bitrix24"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.favoriteBlock.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "block": 6428,
        "meta": {
          "name": "Block with Benefits",
          "section": ["text", "features"],
          "preview": 918273,
          "tpl_code": "bitrix24"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.favoriteBlock.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.landing.favoriteBlock',
            {
                lid: 351,
                block: 6428,
                meta: {
                    name: 'Block with Benefits',
                    section: ['text', 'features'],
                    preview: 918273,
                    tpl_code: 'bitrix24'
                }
            }
        );

        const result = response.getData().result;
        console.info('ID of the saved copy:', result);
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
                'landing.landing.favoriteBlock',
                [
                    'lid' => 351,
                    'block' => 6428,
                    'meta' => [
                        'name' => 'Block with Benefits',
                        'section' => ['text', 'features'],
                        'preview' => 918273,
                        'tpl_code' => 'bitrix24',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error saving block to the block list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.favoriteBlock',
        {
            lid: 351,
            block: 6428,
            meta: {
                name: 'Block with Benefits',
                section: ['text', 'features'],
                preview: 918273,
                tpl_code: 'bitrix24'
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
                console.info('ID of the saved copy:', result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.landing.favoriteBlock',
        [
            'lid' => 351,
            'block' => 6428,
            'meta' => [
                'name' => 'Block with Benefits',
                'section' => ['text', 'features'],
                'preview' => 918273,
                'tpl_code' => 'bitrix24',
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
    "result": 28619,
    "time": {
        "start": 1773958609,
        "finish": 1773958609.566006,
        "duration": 0.5660059452056885,
        "processing": 0,
        "date_start": "2026-03-20T01:16:49+02:00",
        "date_finish": "2026-03-20T01:16:49+02:00",
        "operating_reset_at": 1773959209,
        "operating": 0.21611928939819336
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the saved block copy ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `TYPE_ERROR` | Incorrect type provided for parameter `lid`, `block`, or `meta` ||
|| `LANDING_NOT_EXIST` | Page not found or inaccessible to the current user. The error may refer to both the `lid` page and the page to which the copied block belongs ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found among the blocks of the original page or inaccessible for copying. If the page to which the block belongs is not found, the method usually returns `LANDING_NOT_EXIST` ||
|| `BLOCK_CANT_BE_ADDED` | Failed to create a copy of the block in the block repository ||
|| `BLOCK_WRONG_VERSION` | Block version not supported by the current version of the product ||
|| `ACCESS_DENIED` | Insufficient rights to modify the block ||
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
- [{#T}](./landing-landing-move-block.md)
- [{#T}](./landing-landing-show-block.md)
- [{#T}](./landing-landing-unfavorite-block.md)
- [{#T}](./landing-landing-up-block.md)