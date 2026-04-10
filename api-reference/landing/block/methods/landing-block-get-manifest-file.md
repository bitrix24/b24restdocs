# Get the Manifest File of the Block landing.block.getmanifestfile

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Sites and Stores" section

The method `landing.block.getmanifestfile` returns the original manifest of the block from the file repository.

Unlike [landing.block.getmanifest](./landing-block-get-manifest.md), this method returns the manifest in its original form — as specified in the `.description.php` file. It does not add any service fields, such as `code`, `preview`, or `timestamp`.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code***
[`string`](../../../data-types.md) | The code of the block from the file repository.

You can provide a short code, such as `01.big_with_text`. In this case, the method will search for the block only in the `bitrix` namespace and will automatically add the prefix `bitrix:`.

If you need a block from a different namespace, provide the full code in the format `<namespace>:<code>`.

The code can only contain Latin letters, numbers, and the characters `.` `_` `-` `:` ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "code": "01.big_with_text"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.getmanifestfile.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "code": "01.big_with_text",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.getmanifestfile.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.getmanifestfile',
    		{
    			code: '01.big_with_text'
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
                'landing.block.getmanifestfile',
                [
                    'code' => '01.big_with_text',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting block manifest file: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.getmanifestfile',
        {
            code: '01.big_with_text'
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
        'landing.block.getmanifestfile',
        [
            'code' => '01.big_with_text',
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
    "result": {
        "block": {
            "name": "Cover with Changing Background Images",
            "section": [
                "cover"
            ],
            "dynamic": false
        },
        "cards": {
            ".landing-block-card-img": {
                "name": "Background Image",
                "label": [
                    ".landing-block-card-img"
                ]
            }
        },
        "nodes": {
            ".landing-block-node-small-title": {
                "name": "Subtitle",
                "type": "text"
            },
            ".landing-block-node-title": {
                "name": "Title",
                "type": "text"
            },
            ".landing-block-node-text": {
                "name": "Text",
                "type": "text"
            },
            ".landing-block-node-button": {
                "name": "Button",
                "type": "link"
            },
            ".landing-block-card-img": {
                "name": "Background Image",
                "type": "img",
                "dimensions": {
                    "width": 1920,
                    "height": 1080
                },
                "allowInlineEdit": false,
                "useInDesigner": false,
                "create2xByDefault": false
            }
        },
        "style": {
            "block": {
                "type": [
                    "display"
                ],
                "additional": {
                    "name": "Slider",
                    "attrsType": [
                        "autoplay",
                        "autoplay-speed",
                        "pause-hover",
                        "dots"
                    ]
                }
            },
            "nodes": {
                ".landing-block-node-text-container": {
                    "title": "Set of Elements",
                    "type": [
                        "background-color",
                        "animation"
                    ]
                },
                ".landing-block-node-small-title": {
                    "title": "Subtitle",
                    "type": [
                        "typo",
                        "heading"
                    ]
                },
                ".landing-block-node-title": {
                    "title": "Title",
                    "type": [
                        "typo",
                        "heading"
                    ]
                },
                ".landing-block-node-text": {
                    "title": "Text",
                    "type": "typo"
                },
                ".landing-block-node-button": {
                    "title": "Button",
                    "type": "button"
                },
                ".landing-block-card-img": {
                    "name": "Background Image",
                    "type": [
                        "background-overlay",
                        "height-vh"
                    ]
                }
            }
        },
        "assets": {
            "ext": [
                "landing_carousel"
            ]
        }
    },
    "time": {
        "start": 1774521452,
        "finish": 1774521452.955493,
        "duration": 0.9554929733276367,
        "processing": 0,
        "date_start": "2026-03-26T13:37:32+02:00",
        "date_finish": "2026-03-26T13:37:32+02:00",
        "operating_reset_at": 1774522052,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | The original manifest of the block from the file repository.

The general format of the manifest file is described in the article [Manifest File](../manifest.md).

If the block is not found, the code contains invalid characters, or a local repository block code of the form `repo_<ID>` is provided, the method will return an empty array `[]` without an error ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **block**
[`object`](../../../data-types.md) | Description of the block from the `block` section of the manifest file. Usually contains fields: `name`, `section`, `dynamic`, `type`, `system`, `description`, `version`, `id`, `only_for_license` ||
|| **cards**
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | Description of the block cards, if any ||
|| **nodes**
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | Description of the editable nodes of the block ||
|| **style**
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | Description of the available style settings of the block as declared in the manifest ||
|| **attrs**
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | Description of the customizable attributes of the block, if specified ||
|| **assets**
[`object`](../../../data-types.md) | Resources explicitly declared in the block manifest.

For example, the response may include `assets.ext` if extensions are declared directly in `.description.php` ||
|| **menu**
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | Description of the block menu, if any ||
|#

The `result` may contain other keys. Their composition depends on the specific block and the contents of the manifest.

In nested objects, such as `nodes`, `cards`, and `style.nodes`, the set of fields also depends on the specific block. The method returns this data in its original form, so one block may use the field `title`, while another uses `name`.

Localizable names and labels are returned in the portal's language if they are specified in the manifest through language files.

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough call parameters, missing: code"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required parameter `code` is missing ||
|| `ACCESS_DENIED` | Access denied: the user does not have access to the "Sites and Stores" section ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-get-manifest.md)
- [{#T}](./landing-block-get-repository.md)
- [{#T}](./landing-block-get-content-from-repository.md)