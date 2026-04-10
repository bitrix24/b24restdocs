# Get Block by ID `landing.block.getbyid`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "view" access permission for sites

The method `landing.block.getbyid` returns a single block of the page by its identifier.

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **block***
[`integer`](../../../data-types.md) | Block identifier.

The block identifier can be obtained using the [landing.block.getlist](./landing-block-get-list.md) method. ||
|| **params**
[`object`](../../../data-types.md) | Additional parameters for reading the block [(detailed description)](#params) ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **edit_mode**
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) | If the value is cast to `true`, the method reads the draft version of the page instead of the published version.

Default is `false`. ||
|| **deleted**
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) | If the value is cast to `true`, the method searches for blocks marked as deleted.

Default is `false`. To retrieve a deleted block, you need to pass `deleted: true` along with `edit_mode: true`. ||
|| **get_content**
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) | If the value is cast to `true`, the `result` additionally returns the fields `content`, `css`, and `js`.

Default is `false`. The `content` field contains the prepared HTML of the block along with the system container of the block, not the original saved content. ||
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
        "params": {
          "edit_mode": true,
          "get_content": true
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.getbyid.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "block": 39556,
        "params": {
          "edit_mode": true,
          "get_content": true
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.getbyid.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.getbyid',
    		{
    			block: 39556,
    			params: {
    				edit_mode: true,
    				get_content: true
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
                'landing.block.getbyid',
                [
                    'block' => 39556,
                    'params' => [
                        'edit_mode' => true,
                        'get_content' => true,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting block by ID: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.getbyid',
        {
            block: 39556,
            params: {
                edit_mode: true,
                get_content: true
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
        'landing.block.getbyid',
        [
            'block' => 39556,
            'params' => [
                'edit_mode' => true,
                'get_content' => true,
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
        "id": 39556,
        "lid": 4858,
        "code": "01.big_with_text",
        "name": "Block with Text and Image",
        "active": true,
        "meta": {
            "LID": "2215",
            "FAVORITE_META": "Array",
            "CREATED_BY_ID": "1295",
            "DATE_CREATE": "03/26/2026 11:27:24 am",
            "MODIFIED_BY_ID": "1295",
            "DATE_MODIFY": "03/26/2026 12:23:02 pm",
            "SITE_TYPE": "PAGE",
            "LANDING_TITLE": "",
            "LANDING_TPL_CODE": "bitrix.krayt_otdykh_na_prirode",
            "SITE_TPL_CODE": "empty",
            "XML_ID": "",
            "DESIGNER_MODE": ""
        },
        "content": "<div id=\"b28853\" class=\"block-wrapper block-18-2-two-cols-fix-img-text-button-with-cards\"><section class=\"landing-block g-pt-30 g-pb-30 g-bg-transparent\">...</section></div>",
        "css": [],
        "js": []
    },
    "time": {
        "start": 1774521156,
        "finish": 1774521157.330784,
        "duration": 1.3307840824127197,
        "processing": 1,
        "date_start": "2026-03-26T13:32:36+01:00",
        "date_finish": "2026-03-26T13:32:37+01:00",
        "operating_reset_at": 1774521756,
        "operating": 0.3684520721435547
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Data of the found block [(detailed description)](#result)

The method does not return `result: []`. If the block is not found in the selected version of the page, the method ends with an error. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Block identifier ||
|| **lid**
[`integer`](../../../data-types.md) | Identifier of the page to which the block belongs ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code of the block from the library, for example `01.big_with_text` ||
|| **name**
[`string`](../../../data-types.md) | Name of the block from its manifest ||
|| **active**
[`boolean`](../../../data-types.md) | Indicator of the block's activity.

An active block is displayed on the page. An inactive block is hidden. ||
|| **meta**
[`object`](../../../data-types.md) | Service data of the block and page [(detailed description)](#meta).

All values within `meta` are returned as strings. The format of string dates depends on the language settings of Bitrix24. ||
|| **content**
[`string`](../../../data-types.md) | Prepared HTML of the block. This field is returned only if `params.get_content` is enabled. ||
|| **css**
[`string[]`](../../../data-types.md) | Paths to the CSS files of the block needed for its display.

This field is returned only if `params.get_content` is enabled. If there are no separate CSS resources, an empty array will be returned. ||
|| **js**
[`string[]`](../../../data-types.md) | Paths to the JS files of the block needed for its operation. This field is returned only if `params.get_content` is enabled.

If there are no separate JS resources, an empty array will be returned. ||
|#

### Object meta {#meta}

#|
|| **Name**
`type` | **Description** ||
|| **LID**
[`string`](../../../data-types.md) | Identifier of the block's page in string format.

Duplicates the `lid` field. ||
|| **CREATED_BY_ID**
[`string`](../../../data-types.md) | Identifier of the user who created the block. ||
|| **DATE_CREATE**
[`string`](../../../data-types.md) | Creation date of the block. ||
|| **MODIFIED_BY_ID**
[`string`](../../../data-types.md) | Identifier of the user who last modified the block. ||
|| **DATE_MODIFY**
[`string`](../../../data-types.md) | Date of the last modification of the block. ||
|| **SITE_TYPE**
[`string`](../../../data-types.md) | Type of the site to which the page belongs, for example `PAGE` or `STORE`. ||
|| **LANDING_TITLE**
[`string`](../../../data-types.md) | Title of the page to which the block belongs.

If the title is not filled, an empty string will be returned. ||
|| **LANDING_TPL_CODE**
[`string`](../../../data-types.md) | Code of the page template. ||
|| **SITE_TPL_CODE**
[`string`](../../../data-types.md) | Code of the site template. ||
|| **XML_ID**
[`string`](../../../data-types.md) | External identifier of the block.

If it is not specified, an empty string will be returned. ||
|| **DESIGNER_MODE**
[`string`](../../../data-types.md) | Service field of the designer mode.

In the `landing.block.getbyid` method, it is returned as an empty string. ||
|| **FAVORITE_META**
[`string`](../../../data-types.md) | Service data about saving the block in the user's favorite templates.

If such data is not available, an empty string will be returned. If data is available, in this method the value may come as the string `"Array"`. ||
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
|| `MISSING_PARAMS` | Insufficient call parameters, missing: `block`. ||
|| `ACCESS_DENIED` | The user does not have access to the "Sites and Stores" section. ||
|| `BLOCK_NOT_FOUND` | The block is not found in the selected version of the page, unavailable to the current user, or deleted. ||
|| `TYPE_ERROR` | Internal type mismatch error during method call. ||
|| `SYSTEM_ERROR` | Internal error during method execution. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-get-content.md)
- [{#T}](./landing-block-get-list.md)
- [{#T}](./landing-block-get-manifest.md)
- [{#T}](./landing-block-get-manifest-file.md)