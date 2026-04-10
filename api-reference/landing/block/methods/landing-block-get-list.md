# Get the List of Page Blocks `landing.block.getlist`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" access permission for sites

The method `landing.block.getlist` returns a list of blocks for the selected page.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) \| [`integer[]`](../../../data-types.md) | Identifier of the page or an array of page identifiers.

Page identifiers can be obtained using the method [landing.landing.getList](../../page/methods/landing-landing-get-list.md).

If `lid` is passed as an array, the method combines the blocks of all specified pages into a single list. Each element in the response contains a `lid` field, which indicates the source page.

If an array is passed and at least one page is not found or is unavailable, the entire call results in an error. ||
|| **params**
[`object`](../../../data-types.md) | Additional parameters for reading the list [(detailed description)](#params) ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **edit_mode**
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) | If the value is converted to `true`, the method reads the draft of the page instead of the published version.

By default, it is `false`. Without this parameter, the method reads only published blocks. ||
|| **deleted**
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) | If the value is converted to `true`, the method returns only blocks marked as deleted.

By default, it is `false`. This parameter only affects the selection of blocks. It does not include the search for deleted pages, so for a page in the trash, the method will return an error.

To retrieve deleted blocks, pass `deleted: true` along with `edit_mode: true`. The `deleted` parameter alone does not switch the method to draft mode.

If `edit_mode` is not specified, the method will search for blocks only in the published version of the page. ||
|| **get_content**
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) | If `true` is passed, the method will add the `content`, `css`, and `js` fields to each result element. By default, it is `false`.

The `content` field contains the prepared HTML of the block along with the system container of the block, not the original saved content.

If `edit_mode` is off, the method will return the HTML of the published version of the block. 
If `edit_mode` is on, the method will return the HTML of the draft. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 4858,
        "params": {
          "edit_mode": true,
          "get_content": true
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.getlist.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 4858,
        "params": {
          "edit_mode": true,
          "get_content": true
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.getlist.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.getlist',
    		{
    			lid: 4858,
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
                'landing.block.getlist',
                [
                    'lid' => 4858,
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
        echo 'Error getting block list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.getlist',
        {
            lid: 4858,
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
        'landing.block.getlist',
        [
            'lid' => 4858,
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
    "result": [
        {
            "id": 28181,
            "lid": 2215,
            "code": "0.menu_02",
            "name": "Menu with logo on the left and menu items on the right",
            "active": true,
            "meta": {
                "LID": "2215",
                "FAVORITE_META": "Array",
                "CREATED_BY_ID": "779",
                "DATE_CREATE": "09/13/2024 10:28:08 am",
                "MODIFIED_BY_ID": "1295",
                "DATE_MODIFY": "03/26/2026 12:21:48 pm",
                "SITE_TYPE": "PAGE",
                "LANDING_TITLE": "",
                "LANDING_TPL_CODE": "bitrix.krayt_otdykh_na_prirode",
                "SITE_TPL_CODE": "empty",
                "XML_ID": "",
                "DESIGNER_MODE": ""
            },
            "content": "<div id=\"b2884\" class=\"block-wrapper block-0-menu-02\"><header class=\"landing-block u-header u-header--float u-header--sticky\">...</header></div>",
            "css": [],
            "js": []
        }
    ],
    "time": {
        "start": 1774521039,
        "finish": 1774521039.570632,
        "duration": 0.5706319808959961,
        "processing": 0,
        "date_start": "2026-03-26T13:30:39+03:00",
        "date_finish": "2026-03-26T13:30:39+03:00",
        "operating_reset_at": 1774521639,
        "operating": 0.48987889289855957
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | List of blocks [(detailed description)](#block).

The method may return `result: []` without an error in three cases.

- There are no blocks on the page for the selected reading mode.
- The page has not been published yet and `params.edit_mode` is not enabled.
- The `deleted` filter did not find any matching blocks. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
|#

#### Block Object {#block}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the block. ||
|| **lid**
[`integer`](../../../data-types.md) | Identifier of the page to which the block belongs. ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code of the block from the library, for example `0.menu_02`. ||
|| **name**
[`string`](../../../data-types.md) | Name of the block from its manifest. ||
|| **active**
[`boolean`](../../../data-types.md) | Indicator of the block's activity.

An active block is displayed on the page. An inactive block is hidden. ||
|| **meta**
[`object`](../../../data-types.md) | Service data of the block and page [(detailed description)](#meta).

All values within `meta` are returned as strings. The format of string dates depends on the language settings of the account. ||
|| **content**
[`string`](../../../data-types.md) | Prepared HTML of the block. This field is returned only if `params.get_content` is enabled.

If `params.edit_mode = false`, the method will return the HTML of the published version of the block. 
If `params.edit_mode = true`, the method will return the HTML of the draft. ||
|| **css**
[`string[]`](../../../data-types.md) | Paths to the CSS files of the block needed for its display.

This field is returned only if `params.get_content` is enabled. If there are no separate CSS resources, an empty array will be returned. ||
|| **js**
[`string[]`](../../../data-types.md) | Paths to the JS files of the block needed for its operation.

This field is returned only if `params.get_content` is enabled. If there are no separate JS resources, an empty array will be returned. ||
|#

#### Meta Object {#meta}

#|
|| **Name**
`type` | **Description** ||
|| **LID**
[`string`](../../../data-types.md) | Identifier of the block's page in string format.

Duplicates the top-level field `lid`. ||
|| **CREATED_BY_ID**
[`string`](../../../data-types.md) | Identifier of the user who created the block. ||
|| **DATE_CREATE**
[`string`](../../../data-types.md) | Date of block creation.

The format depends on the language settings of the account. ||
|| **MODIFIED_BY_ID**
[`string`](../../../data-types.md) | Identifier of the user who last modified the block. ||
|| **DATE_MODIFY**
[`string`](../../../data-types.md) | Date of the last modification of the block.

The format depends on the language settings of the account. ||
|| **SITE_TYPE**
[`string`](../../../data-types.md) | Type of the site to which the page belongs, for example `PAGE` or `STORE`. ||
|| **LANDING_TITLE**
[`string`](../../../data-types.md) | Title of the page to which the block belongs.

If the title is not filled in, an empty string will be returned. ||
|| **LANDING_TPL_CODE**
[`string`](../../../data-types.md) | Code of the page template. ||
|| **SITE_TPL_CODE**
[`string`](../../../data-types.md) | Code of the site template. ||
|| **XML_ID**
[`string`](../../../data-types.md) | External identifier of the block.

If it is not specified, an empty string will be returned. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "LANDING_NOT_EXIST",
    "error_description": "Landing not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required top-level parameter `lid` is missing. ||
|| `LANDING_NOT_EXIST` | The page is not found, deleted, or unavailable to the current user. ||
|| `ACCESS_DENIED` | The user does not have permission to view sites. ||
|| `TYPE_ERROR` | Internal type mismatch error when calling the method. ||
|| `SYSTEM_ERROR` | Internal error during method execution. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-get-content.md)
- [{#T}](./landing-block-get-by-id.md)
- [{#T}](./landing-block-get-manifest.md)
- [{#T}](./landing-block-get-manifest-file.md)