# Add Block to Page `landing.landing.addblock`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for the site

The method `landing.landing.addblock` adds a new block to the page and returns the identifier of the created block.

If the page is already published, the new block will be visible to visitors after the "Publish Changes" command in the interface or after calling the method [landing.landing.publication](../methods/landing-landing-publication.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of landings. It is not related to the REST scope `landing` in the method name.

For pages of types `PAGE`, `STORE`, and `SMN`, this parameter does not need to be passed. For pages in the scopes `GROUP`, `KNOWLEDGE`, and `MAINPAGE`, pass the same value for `scope`. The rules for selecting the value are described in the article [Working with Site Types and Scopes](../../types.md) ||
|| **lid***
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the method [landing.landing.getList](../methods/landing-landing-get-list.md), as well as from the results of the methods [landing.landing.add](../methods/landing-landing-add.md), [landing.landing.addByTemplate](../methods/landing-landing-add-by-template.md), and [landing.landing.copy](../methods/landing-landing-copy.md) ||
|| **fields***
[`object`](../../../data-types.md) | Set of parameters for the new block [(detailed description)](#fields) ||
|| **preventHistory**
[`boolean`](../../../data-types.md) | If `true`, the method does not add the action to the page change history. Default is `false` ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../../data-types.md) | Symbolic code of the block from the repository.

The code can be obtained using the method [landing.block.getrepository](../../block/methods/landing-block-get-repository.md): for `fields.CODE`, take the key of the element from `result.items`, for example `02.three_cols_big_1` or `repo_385`.

For a block registered by the application through [landing.repo.register](../../user-blocks/landing-repo-register.md), use a value in the form of `repo_<ID>`.

The availability of the code depends on the type of page and the top-level parameter `scope`, if it is passed. If you obtain the code via `landing.block.getrepository`, use the same `scope` as in `landing.landing.addblock`.

If the parameter is not passed or passed as an empty string, the method will return an error ||
|| **AFTER_ID**
[`integer`](../../../data-types.md) | Identifier of the block after which the new block should be inserted.

The block identifier can be obtained using the method [landing.block.getList](../../block/methods/landing-block-get-list.md).

If you pass the identifier of an existing block on the page, the new block will be added immediately after it. If the parameter is not passed, the new block will be added to the beginning of the page.

If `AFTER_ID` is passed but there is no block with that identifier on the page, there will be no separate error. In this case, the new block will also be added to the beginning of the page ||
|| **ACTIVE**
[`string`](../../../data-types.md) | Flag indicating the activity of the new block. Possible values:

`Y` — block is active
`N` — block is inactive

Default is `Y` ||
|| **CONTENT**
[`string`](../../../data-types.md) | HTML content of the block. Allows replacing the standard content of the block with your own HTML code. The block code must still be available in the repository for the current type of page and `scope`. Before saving, the value is cleaned and validated ||
|| **RETURN_CONTENT**
[`string`](../../../data-types.md) | If `Y` is passed, after adding the block, the method will return not only its identifier but also the block data [(detailed description)](#result-content). For any other value, the method will return only the block identifier ||
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
        "fields": {
          "CODE": "02.three_cols_big_1",
          "AFTER_ID": 6428,
          "ACTIVE": "Y"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.addblock.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "fields": {
          "CODE": "02.three_cols_big_1",
          "AFTER_ID": 6428,
          "ACTIVE": "Y"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.addblock.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.addblock',
    		{
    			lid: 351,
    			fields: {
    				CODE: '02.three_cols_big_1',
    				AFTER_ID: 6428,
    				ACTIVE: 'Y'
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
                'landing.landing.addblock',
                [
                    'lid' => 351,
                    'fields' => [
                        'CODE' => '02.three_cols_big_1',
                        'AFTER_ID' => 6428,
                        'ACTIVE' => 'Y',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.addblock',
        {
            lid: 351,
            fields: {
                CODE: '02.three_cols_big_1',
                AFTER_ID: 6428,
                ACTIVE: 'Y'
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
        'landing.landing.addblock',
        [
            'lid' => 351,
            'fields' => [
                'CODE' => '02.three_cols_big_1',
                'AFTER_ID' => 6428,
                'ACTIVE' => 'Y',
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
    "result": 28597,
    "time": {
        "start": 1773923439,
        "finish": 1773923439.57418,
        "duration": 0.5741798877716064,
        "processing": 0,
        "date_start": "2026-03-19T15:30:39+02:00",
        "date_finish": "2026-03-19T15:30:39+02:00",
        "operating_reset_at": 1773924039,
        "operating": 0.10522103309631348
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) \| [`object`](../../../data-types.md) | Identifier of the created block. If `fields.RETURN_CONTENT = 'Y'` is passed, the method will return the block object [(detailed description)](#result-content) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

### Result Object when `RETURN_CONTENT = 'Y'` {#result-content}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the created block ||
|| **sections**
[`string`](../../../data-types.md) | Codes of the block sections from the manifest, concatenated into a single string with commas ||
|| **active**
[`boolean`](../../../data-types.md) | Indicator of the block's activity ||
|| **access**
[`string`](../../../data-types.md) | Access level to the block ||
|| **anchor**
[`string`](../../../data-types.md) | Local anchor of the block. By default, the new block receives an anchor in the form `b<ID>`, for example `b28597` ||
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
[`array`](../../../data-types.md) | List of CSS resources for the block ||
|| **js**
[`array`](../../../data-types.md) | List of JS resources for the block. For standard blocks from the repository, this field may contain connections. For REST blocks of the form `repo_<ID>` when `RETURN_CONTENT = 'Y'`, this field returns an empty array ||
|| **assetStrings**
[`array`](../../../data-types.md) | Initialization strings for resources that need to be added to the page ||
|| **lang**
[`array`](../../../data-types.md) \| [`object`](../../../data-types.md) | Language messages of connected extensions. If messages exist, the field returns as an object in key-value form. If there are no additional messages, it may return an empty array ||
|| **manifest**
[`object`](../../../data-types.md) | Manifest of the block. The format is described in the section [Block Manifest](../../block/manifest.md) ||
|| **dynamicParams**
[`array`](../../../data-types.md) | Parameters of the dynamic block ||
|| **requiredUserAction**
[`array`](../../../data-types.md) | The field contains the same data as `manifest.requiredUserAction`. It is returned only when the user must perform an additional action on the client side after adding the block. The specific data in the field depends on the block's manifest ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "BLOCK_CANT_BE_ADDED",
    "error_description": "Cannot add block because it is not intended for this type of page."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required top-level parameter `lid` or `fields` is missing ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found ||
|| `ACCESS_DENIED` | User does not have permission to call the method or does not have permission to edit the page ||
|| `BLOCK_CANT_BE_ADDED` | Code from `fields.CODE` is not available in the repository, this block cannot be added to the current type of page, or the passed `scope` has restricted the set of available blocks. The same error is returned if `fields.CODE` is not passed or passed as an empty string ||
|| `BLOCK_WRONG_VERSION` | The version of the block from the repository is higher than the version of the installed landing module ||
|| `BLOCK_NOT_FOUND` | Content for the block not found, including if an empty `fields.CONTENT` is passed ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-copy-block.md)
- [{#T}](./landing-landing-delete-block.md)
- [{#T}](./landing-landing-down-block.md)
- [{#T}](./landing-landing-move-block.md)
- [{#T}](./landing-landing-show-block.md)
- [{#T}](./landing-landing-up-block.md)
- [{#T}](../methods/landing-landing-publication.md)