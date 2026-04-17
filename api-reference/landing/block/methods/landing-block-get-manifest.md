# Get the Manifest of the `landing.block.getmanifest` Method

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" access permission for pages

The `landing.block.getmanifest` method returns a prepared manifest of the block placed on the page.

It does not return the original file but rather the prepared data for a specific block. For example, localized titles, fields such as `code`, `preview`, `assets`, `timestamp`, and `callbacks`.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of the landing. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid*** 
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the [landing.landing.getlist](../../page/methods/landing-landing-get-list.md) method ||
|| **block*** 
[`integer`](../../../data-types.md) | Block identifier. The block must belong to the page `lid` in the selected version of the page.

The block identifier can be obtained using the [landing.block.getlist](./landing-block-get-list.md) method ||
|| **params**
[`object`](../../../data-types.md) | Additional parameters for reading the manifest [(detailed description)](#params) ||
|#

### Parameter params {#params}

#| 
|| **Name**
`type` | **Description** ||
|| **edit_mode**
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) | If the value is cast to `true`, the method reads the draft of the page instead of the published version. Default is `false`.

Without this parameter, the method searches for the block only in the published version of the page. ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 4858,
        "block": 39556,
        "params": {
          "edit_mode": true
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.getmanifest.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 4858,
        "block": 39556,
        "params": {
          "edit_mode": true
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.getmanifest.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.getmanifest',
    		{
    			lid: 4858,
    			block: 39556,
    			params: {
    				edit_mode: true
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
                'landing.block.getmanifest',
                [
                    'lid' => 4858,
                    'block' => 39556,
                    'params' => [
                        'edit_mode' => true,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting block manifest: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.getmanifest',
        {
            lid: 4858,
            block: 39556,
            params: {
                edit_mode: true
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
        'landing.block.getmanifest',
        [
            'lid' => 4858,
            'block' => 39556,
            'params' => [
                'edit_mode' => true,
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
    "result": {
        "block": {
            "name": "Form on a light background in the center",
            "section": [
                "forms"
            ],
            "dynamic": false,
            "subtype": "form",
            "attrsFormDescription": "<a href=\"/crm/webform/\" target=\"_blank\">Configure CRM forms</a>"
        },
        "nodes": {
            "#wrapper": {
                "type": "styleimg",
                "code": "#wrapper"
            }
        },
        "style": {
            "block": {
                "type": [
                    "block-default",
                    "block-border"
                ]
            },
            "nodes": {
                ".bitrix24forms": {
                    "type": "crm-form"
                }
            }
        },
        "assets": {
            "css": [],
            "js": [],
            "ext": [
                "landing_form"
            ],
            "class": [],
            "callbacks": []
        },
        "timestamp": 1751467642,
        "callbacks": {
            "afteradd": {}
        },
        "attrs": {
            ".bitrix24forms": [
                {
                    "name": "Embed form flag",
                    "attribute": "data-b24form-embed",
                    "type": "string",
                    "hidden": true
                },
                {
                    "name": "Form design",
                    "attribute": "data-b24form-design",
                    "type": "string",
                    "hidden": true
                },
                {
                    "name": "Form from connector flag",
                    "attribute": "data-b24form-connector",
                    "type": "string",
                    "hidden": true
                },
                {
                    "name": "CRM form",
                    "attribute": "data-b24form",
                    "items": [
                        {
                            "name": "Feedback form",
                            "value": "#crmFormInline3"
                        },
                        {
                            "name": "Contact details with comment",
                            "value": "#crmFormInline39"
                        }
                    ],
                    "type": "list"
                },
                {
                    "name": "Form design",
                    "attribute": "data-b24form-use-style",
                    "type": "list",
                    "items": [
                        {
                            "name": "Use block design",
                            "value": "Y"
                        },
                        {
                            "name": "Use CRM form design",
                            "value": "N"
                        }
                    ]
                }
            ]
        },
        "cards": [],
        "menu": [],
        "namespace": "bitrix",
        "code": "33.13.form_2_light_no_text",
        "preview": "/bitrix/blocks/bitrix/33.13.form_2_light_no_text/preview.jpg"
    },
    "time": {
        "start": 1774521323,
        "finish": 1774521323.212432,
        "duration": 0.2124319076538086,
        "processing": 0,
        "date_start": "2026-03-26T13:35:23+01:00",
        "date_finish": "2026-03-26T13:35:23+01:00",
        "operating_reset_at": 1774521923,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The prepared manifest of the block [(detailed description)](#result). The general format of the manifest is described in the article [Manifest File](../manifest.md).

Empty sections of the manifest may return as empty arrays `[]`, even if they usually contain an object with keys ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

### Object result {#result}

#| 
|| **Name**
`type` | **Description** ||
|| **block**
[`object`](../../../data-types.md) | Main properties of the block from the manifest [(detailed description)](#block) ||
|| **cards**
[`object`](../../../data-types.md) | Description of the block cards, if any.

If there are no cards, it may return an empty array `[]` ||
|| **nodes**
[`object`](../../../data-types.md) | Description of the editable nodes of the block.

For each node, the method additionally adds a `code` key with the node selector.

For nodes with a separate handler in the editor, the method also adds a `handler` key with the name of that handler. If there are no nodes, it may return an empty array `[]` ||
|| **attrs**
[`object`](../../../data-types.md) | Description of customizable attributes of the block, if any.

If there are no attributes, it may return an empty array `[]` ||
|| **menu**
[`object`](../../../data-types.md) | Description of the block menu, if any.

If there is no menu, it may return an empty array `[]` ||
|| **style**
[`object`](../../../data-types.md) | Description of available style settings for the block.

If the original manifest does not separate styles into `style.block` and `style.nodes`, the method will sort them into these sections itself ||
|| **namespace**
[`string`](../../../data-types.md) | Namespace of the block.

For built-in blocks in Bitrix24, this is usually `bitrix`. For blocks from applications, the value is set by the application, so it may differ or be empty ||
|| **code**
[`string`](../../../data-types.md) | Block code ||
|| **preview**
[`string`](../../../data-types.md) | Relative path to the preview file `preview.jpg`.

If the preview file is missing, an empty string will be returned. For blocks registered via REST API or applications, this field returns an empty string ||
|| **assets**
[`object`](../../../data-types.md) | Block resources [(detailed description)](#assets) ||
|| **timestamp**
[`integer`](../../../data-types.md) | Time of the base manifest update in Unix Timestamp format.

For a local block, this is the time of the file `block.php` modification. For a block from an application, this is the time of the last update of the block in the application ||
|| **callbacks**
[`object`](../../../data-types.md) | Callback handlers from the block manifest.

The names of the handlers are converted to lowercase by the method ||
|#

The `result` may contain other keys from the original manifest. Their composition depends on the specific block.

### Object block {#block}

#| 
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Name of the block ||
|| **section**
[`string[]`](../../../data-types.md) | Sections to which the block belongs ||
|| **dynamic**
[`boolean`](../../../data-types.md) | Indicator of a dynamic block ||
|| **subtype**
[`string`](../../../data-types.md) \| [`string[]`](../../../data-types.md) | Subtype of the block, if specified ||
|#

In the `block` object, there may be other fields of the manifest. Their set depends on the specific block.

### Object assets {#assets}

#| 
|| **Name**
`type` | **Description** ||
|| **css**
[`string[]`](../../../data-types.md) | List of CSS resources for the block.

This includes paths from the manifest and any automatically found local `style.css`, if available.

For design local blocks, `design_style.css` is automatically included instead of `style.css` ||
|| **js**
[`string[]`](../../../data-types.md) | List of JS resources for the block.

This includes paths from the manifest and any automatically found local `script.js`, if available ||
|| **ext**
[`string[]`](../../../data-types.md) | List of client extensions for the block.

For REST blocks, the method returns only extensions from the allowed list ||
|| **class**
[`string[]`](../../../data-types.md) | Service paths to PHP classes of the block on the server.

For most regular calls, this array is empty or not needed for client code ||
|| **callbacks**
[`array`](../../../data-types.md) | List of callback functions from the `assets` section of the manifest, if declared.

Unlike `result.callbacks`, this field pertains to the resources of the block, not to the handlers of the block itself ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "BLOCK_NOT_FOUND",
    "error_description": "Block not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid` or `block` is missing ||
|| `ACCESS_DENIED` | User does not have permission to view the page ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found, deleted, or unavailable to the current user ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found on the page in the selected version ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-get-content.md)
- [{#T}](./landing-block-get-list.md)
- [{#T}](./landing-block-get-by-id.md)
- [{#T}](./landing-block-get-manifest-file.md)