# Add a Custom Block to the Repository landing.repo.register

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with View access permission in the Sites section

The method `landing.repo.register` adds a new custom block to the repository.

The method updates the block if it already exists with the same code for the current application. If it does not exist, it creates a new one.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code**^*^
[`string`](../../data-types.md) | Unique block code ||
|| **fields**^*^
[`object`](../../data-types.md) | Block fields [more details](#fields) ||
|| **manifest**
[`object`](../../data-types.md) | Block manifest.

See the structure of the manifest in the method [landing.block.getManifestFile](../block/methods/landing-block-get-manifest-file.md) and in the section [Manifest File](../block/manifest.md) ||
|#

### Type fields {#fields}

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME**^*^
[`string`](../../data-types.md) | Block name ||
|| **CONTENT**^*^
[`string`](../../data-types.md) | HTML content of the block. It undergoes a sanitize check before saving ||
|| **SECTIONS**^*^
[`string`](../../data-types.md) | Block categories as a comma-separated string, e.g., `cover,about` ||
|| **PREVIEW**^*^
[`string`](../../data-types.md) | Block preview URL ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Block description ||
|| **ACTIVE**
[`string`](../../data-types.md) | Block activity (`Y`/`N`) ||
|| **SITE_TEMPLATE_ID**
[`string`](../../data-types.md) | Binding of the block to the site template ||
|| **RESET**
[`string`](../../data-types.md) | If `Y` is passed, the method registers updates for blocks added to pages with the code `repo_<ID>` ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of registering a block where:
- `code` — unique block code `myblockx`
- `fields` — main parameters of the block (`NAME`, `DESCRIPTION`, `SECTIONS`, `PREVIEW`, `CONTENT`)
- `manifest` — basic manifest of the block with the description of `block` and `nodes`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "code": "myblockx",
        "fields": {
          "NAME": "Test block",
          "DESCRIPTION": "Just try!",
          "SECTIONS": "cover,about",
          "PREVIEW": "https://www.bitrix24.com/images/b24_screen.png",
          "CONTENT": "<section class=\"landing-block\"><div class=\"container\">Test</div></section>"
        },
        "manifest": {
          "block": {"name": "Test block"},
          "nodes": {
            ".landing-block-node-text": {"name": "Text", "type": "text"}
          }
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.repo.register.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "code": "myblockx",
        "fields": {
          "NAME": "Test block",
          "SECTIONS": "cover,about",
          "PREVIEW": "https://www.bitrix24.com/images/b24_screen.png",
          "CONTENT": "<section class=\"landing-block\"><div class=\"container\">Test</div></section>"
        },
        "manifest": {
          "block": {"name": "Test block"}
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.repo.register.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.repo.register',
    		{
    			code: 'myblockx',
    			fields: {
    				NAME: 'Test block',
    				DESCRIPTION: 'Just try!',
    				SECTIONS: 'cover,about',
    				PREVIEW: 'https://www.bitrix24.com/images/b24_screen.png',
    				CONTENT: '<section class="landing-block"><div class="container">Test</div></section>'
    			},
    			manifest: {
    				block: { name: 'Test block' },
    				nodes: {
    					'.landing-block-node-text': { name: 'Text', type: 'text' }
    				}
    			}
    		}
    	);

    	console.info(response.getData().result);
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
                'landing.repo.register',
                [
                    'code' => 'myblockx',
                    'fields' => [
                        'NAME' => 'Test block',
                        'DESCRIPTION' => 'Just try!',
                        'SECTIONS' => 'cover,about',
                        'PREVIEW' => 'https://www.bitrix24.com/images/b24_screen.png',
                        'CONTENT' => '<section class="landing-block"><div class="container">Test</div></section>',
                    ],
                    'manifest' => [
                        'block' => ['name' => 'Test block'],
                        'nodes' => [
                            '.landing-block-node-text' => ['name' => 'Text', 'type' => 'text'],
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling landing.repo.register: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repo.register',
        {
            code: 'myblockx',
            fields: {
                NAME: 'Test block',
                DESCRIPTION: 'Just try!',
                SECTIONS: 'cover,about',
                PREVIEW: 'https://www.bitrix24.com/images/b24_screen.png',
                CONTENT: '<section class="landing-block"><div class="container">Test</div></section>'
            },
            manifest: {
                block: { name: 'Test block' },
                nodes: {
                    '.landing-block-node-text': { name: 'Text', type: 'text' }
                }
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
        'landing.repo.register',
        [
            'code' => 'myblockx',
            'fields' => [
                'NAME' => 'Test block',
                'DESCRIPTION' => 'Just try!',
                'SECTIONS' => 'cover,about',
                'PREVIEW' => 'https://www.bitrix24.com/images/b24_screen.png',
                'CONTENT' => '<section class="landing-block"><div class="container">Test</div></section>',
            ],
            'manifest' => [
                'block' => ['name' => 'Test block'],
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
    "result": 7,
    "time": {
        "start": 1774879194,
        "finish": 1774879194.526507,
        "duration": 0.5265069007873535,
        "processing": 0,
        "date_start": "2026-03-30T16:59:54+02:00",
        "date_finish": "2026-03-30T16:59:54+02:00",
        "operating_reset_at": 1774879794,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the added or updated block in the repository ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MANIFEST_INTERSECT_IMG",
    "error_description": "The editable manifest element \".landing-block-node-text\" cannot have a style type of \"Image\". Change the style type."
}
```

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "The value of an argument 'code' has an invalid type",
    "argument": "code"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | Insufficient permissions | User did not pass general access checks ||
|| `ERROR_ARGUMENT` | Invalid argument type | An argument was passed in an incorrect type. The name of the argument is returned in the `argument` field ||
|| `MISSING_PARAMS` | Insufficient call parameters | A required parameter (`code` or `fields`) was not passed ||
|| `REQUIRED_FIELD_NO_EXISTS` | Missing required field | One of the fields was not passed: `NAME`, `CONTENT`, `SECTIONS`, `PREVIEW` ||
|| `MANIFEST_INTERSECT_IMG` | Type conflict in the manifest | A `node` and a style with type `background`, `block-default`, or `block-border` are set for one selector ||
|| `CONTENT_IS_BAD` | Unsafe block content | The `fields.CONTENT` field did not pass the sanitize check ||
|| `PRESET_CONTENT_IS_BAD` | Unsafe preset content | Unsafe content in `manifest.cards[*].presets[*]` ||
|| `UNSUPPORTED_BLOCK_TYPE` | Unsupported block type | The method code checks the restriction on `MAINPAGE` in `manifest.block.type` ||
|| `UNSUPPORTED_BLOCK_SUBTYPE` | Unsupported block subtype | `widget` was passed in `manifest.block.subtype` ||
|| `insufficient_scope` | Insufficient scope for the token | The token does not contain the `landing` scope ||
|| `SYSTEM_ERROR` | Internal error | Execution error on the server side ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-repo-check-content.md)
- [{#T}](./landing-repo-get-list.md)
- [{#T}](./landing-repo-unregister.md)
- [{#T}](./index.md)