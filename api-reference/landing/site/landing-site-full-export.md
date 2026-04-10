# Exporting the site landing.site.fullExport

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with "export" access permission for sites

The method `landing.site.fullExport` exports the site and its pages into an array for subsequent import, for example, via [landing.demos.register](../demos/landing-demos-register.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Site identifier.

The site identifier can be obtained using the method [landing.site.getList](./landing-site-get-list.md) or from the result of the method [landing.site.add](./landing-site-add.md) ||
|| **params**
[`object`](../../data-types.md) | Additional export parameters [(detailed description)](#params) ||
|#

Pages within the export are selected in the order of `ID ASC` and returned in a single response.

### Parameter params {#params}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **edit_mode**
[`string`](../../data-types.md) | Hook export mode. When set to `Y`, edit mode is enabled. The default is standard mode ||
|| **scope**
[`string`](../../data-types.md) | Internal scope of the landings. It is not related to the REST scope `landing` or `landing_cloud` in the method name. 

For `GROUP`, `KNOWLEDGE`, and `MAINPAGE`, the value of `scope` must correspond to the type of the exported site ||
|| **hooks_disable**
[`string[]`](../../data-types.md) | Codes of additional fields to be excluded from `ADDITIONAL_FIELDS` at the site and page level. 

If the parameter is not provided, an empty array is used. Regardless of the input, the method always additionally excludes `B24BUTTON_CODE` and `FAVICON_PICTURE` ||
|| **code**
[`string`](../../data-types.md) | Code of the exported site. If not provided, the current site code is used without leading or trailing `/`. 

Only Latin letters and digits are allowed (`[a-z0-9]`, without separators) ||
|| **name**
[`string`](../../data-types.md) | Name of the site in the export. If not provided, the current site name is used ||
|| **description**
[`string`](../../data-types.md) | Description of the site in the export. If not provided, the current site description is used ||
|| **preview**
[`string`](../../data-types.md) | URL of the main preview image. Defaults to an empty string ||
|| **preview2x**
[`string`](../../data-types.md) | URL of the enlarged preview image. Defaults to an empty string ||
|| **preview3x**
[`string`](../../data-types.md) | URL of the retina preview. Defaults to an empty string ||
|| **preview_url**
[`string`](../../data-types.md) | URL of the template preview. Defaults to an empty string ||
|#

The parameters `name`, `description`, `preview`, `preview2x`, `preview3x`, and `preview_url` apply to the page data only if the site contains a single page. If there are multiple pages, these parameters set the top level of the site export, and the page data is taken from the current values of each page.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 326,
        "params": {
          "edit_mode": "Y",
          "code": "myfirstsite2026",
          "name": "Auto Repair Shop",
          "description": "Website for auto service",
          "preview_url": "https://example.com/previews/myfirstsite2026"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.fullExport.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 326,
        "params": {
          "edit_mode": "Y",
          "code": "myfirstsite2026",
          "name": "Auto Repair Shop",
          "description": "Website for auto service",
          "preview_url": "https://example.com/previews/myfirstsite2026"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.fullExport.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.fullExport',
    		{
    			id: 326,
    			params: {
    				edit_mode: 'Y',
    				code: 'myfirstsite2026',
    				name: 'Auto Repair Shop',
    				description: 'Website for auto service',
    				preview_url: 'https://example.com/previews/myfirstsite2026'
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
                'landing.site.fullExport',
                [
                    'id' => 326,
                    'params' => [
                        'edit_mode' => 'Y',
                        'code' => 'myfirstsite2026',
                        'name' => 'Auto Repair Shop',
                        'description' => 'Website for auto service',
                        'preview_url' => 'https://example.com/previews/myfirstsite2026',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error exporting site: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.fullExport',
        {
            id: 326,
            params: {
                edit_mode: 'Y',
                code: 'myfirstsite2026',
                name: 'Auto Repair Shop',
                description: 'Website for auto service',
                preview_url: 'https://example.com/previews/myfirstsite2026'
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
        'landing.site.fullExport',
        [
            'id' => 326,
            'params' => [
                'edit_mode' => 'Y',
                'code' => 'myfirstsite2026',
                'name' => 'Auto Repair Shop',
                'description' => 'Website for auto service',
                'preview_url': 'https://example.com/previews/myfirstsite2026',
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
        "charset": "UTF-8",
        "code": "myfirstsite2026",
        "site_code": "/dgiy8z1opr/",
        "name": "Auto Repair Shop",
        "description": "Website for auto service",
        "preview": "",
        "preview2x": "",
        "preview3x": "",
        "preview_url": "https://example.com/previews/myfirstsite2026",
        "show_in_list": "Y",
        "type": "page",
        "version": 3,
        "fields": {
            "ADDITIONAL_FIELDS": {
                "COOKIES_USE": "N",
                "B24BUTTON_COLOR": "site",
                "BACKGROUND_PICTURE": "https://cdn.com.bitrix24.com/.../15_1x.jpg",
                "THEME_USE": "Y"
            },
            "TITLE": "Auto Repair Shop",
            "LANDING_ID_INDEX": "myfirstsite2026",
            "LANDING_ID_404": "0"
        },
        "layout": [],
        "folders": [],
        "syspages": [],
        "items": {
            "myfirstsite2026": {
                "old_id": "2213",
                "code": "myfirstsite2026",
                "name": "Auto Repair Shop",
                "description": "Website for auto service",
                "preview": "",
                "preview2x": "",
                "preview3x": "",
                "preview_url": "https://example.com/previews/myfirstsite2026",
                "show_in_list": "Y",
                "type": "page",
                "version": 3,
                "fields": {
                    "TITLE": "Auto Repair Shop",
                    "RULE": null,
                    "ADDITIONAL_FIELDS": {
                        "B24BUTTON_USE": "N",
                        "METAOG_TITLE": "Vacation Home in Germany Villa Randu",
                        "THEME_USE": "N"
                    }
                },
                "layout": [],
                "items": {
                    "#block28175": {
                        "old_id": 28175,
                        "code": "0.menu_02",
                        "access": "X",
                        "anchor": "b2884",
                        "nodes": {
                            ".landing-block-node-menu-list-item-link": [
                                {
                                    "href": "#cottages",
                                    "target": "_self",
                                    "text": "Villas"
                                }
                            ]
                        },
                        "style": {
                            ".navbar": [
                                "navbar navbar-expand-lg p-0 g-px-15 u-navbar-align-right"
                            ]
                        },
                        "attrs": {
                            ".navbar-collapse": [
                                {
                                    "id": "navBar2884"
                                }
                            ]
                        }
                    }
                }
            }
        }
    },
    "time": {
        "start": 1773161828.471138,
        "finish": 1773161828.871144,
        "duration": 0.4000060558319092,
        "processing": 0.10344195365905762,
        "date_start": "2026-03-10T19:57:08+02:00",
        "date_finish": "2026-03-10T19:57:08+02:00",
        "operating_reset_at": 1773162428,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Fields of the exported site. If the site is not found in the selection or there are no available pages for export, an empty array `[]` is returned [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **charset**
[`string`](../../data-types.md) | Encoding of the exported set, usually `UTF-8` ||
|| **code**
[`string`](../../data-types.md) | Code of the site in the export ||
|| **site_code**
[`string`](../../data-types.md) | Original site code ||
|| **name**
[`string`](../../data-types.md) | Name of the site ||
|| **description**
[`string`](../../data-types.md) | Description of the site ||
|| **preview**
[`string`](../../data-types.md) | URL of the main preview image ||
|| **preview2x**
[`string`](../../data-types.md) | URL of the enlarged preview image ||
|| **preview3x**
[`string`](../../data-types.md) | URL of the retina preview ||
|| **preview_url**
[`string`](../../data-types.md) | URL of the preview ||
|| **show_in_list**
[`string`](../../data-types.md) | Flag for display in the template list `Y/N` ||
|| **type**
[`string`](../../data-types.md) | Type of the site in lowercase ||
|| **version**
[`integer`](../../data-types.md) | Version of the export format. The method returns `3` ||
|| **fields**
[`object`](../../data-types.md) | Site fields [(detailed description)](#result-fields) ||
|| **layout**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Site template data. If the template is not linked, an empty array `[]` is returned [(detailed description)](#result-layout) ||
|| **folders**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Groups of pages by folders in the format `{"<folder_page_code>": ["<page_code_1>", "..."]}`. 

If there are no folders, `[]` is returned ||
|| **syspages**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | System pages in the format `{"<system_page_type>": "<page_code>"}`. 

If system pages are not defined, `[]` is returned ||
|| **items**
[`object`](../../data-types.md) | Export of the site's pages, where the object key is the page code [(detailed description)](#result-items) ||
|#

### Object fields {#result-fields}

#|
|| **Name**
`type` | **Description** ||
|| **ADDITIONAL_FIELDS**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Additional fields of the site after filtering by `hooks_disable` ||
|| **TITLE**
[`string`](../../data-types.md) | Site title ||
|| **LANDING_ID_INDEX**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Code or identifier of the main page ||
|| **LANDING_ID_404**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Code or identifier of the `404` page ||
|#

### Object layout {#result-layout}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../../data-types.md) | XML_ID of the site template ||
|| **ref**
[`string[]`](../../data-types.md) | Codes of pages associated with the site template ||
|#

### Object items {#result-items}

#|
|| **Name**
`type` | **Description** ||
|| **`<page_code>`**
[`object`](../../data-types.md) | Export of an individual page [(detailed description)](#page-item) ||
|#

### Object page {#page-item}

#|
|| **Name**
`type` | **Description** ||
|| **old_id**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Original page identifier ||
|| **code**
[`string`](../../data-types.md) | Page code in the export ||
|| **name**
[`string`](../../data-types.md) | Page name ||
|| **description**
[`string`](../../data-types.md) | Page description ||
|| **preview**
[`string`](../../data-types.md) | URL of the main preview image of the page ||
|| **preview2x**
[`string`](../../data-types.md) | URL of the enlarged preview image of the page ||
|| **preview3x**
[`string`](../../data-types.md) | URL of the retina preview of the page ||
|| **preview_url**
[`string`](../../data-types.md) | URL of the page preview ||
|| **show_in_list**
[`string`](../../data-types.md) | Flag for displaying the page `Y/N` ||
|| **type**
[`string`](../../data-types.md) | Type of the page site in lowercase ||
|| **version**
[`integer`](../../data-types.md) | Version of the page export format ||
|| **fields**
[`object`](../../data-types.md) | Page fields [(detailed description)](#page-fields) ||
|| **layout**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Page template data. 

If the template is not linked, an empty array `[]` is returned [(detailed description)](#page-layout) ||
|| **items**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Page blocks [(detailed description)](#page-blocks) ||
|#

### Object fields of the page {#page-fields}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../../data-types.md) | Page title ||
|| **RULE**
[`string`](../../data-types.md) \| `null` | Page routing rule ||
|| **ADDITIONAL_FIELDS**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Additional fields of the page after filtering by `hooks_disable` ||
|#

### Object layout of the page {#page-layout}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../../data-types.md) | XML_ID of the page template ||
|| **ref**
[`string[]`](../../data-types.md) | Codes of pages associated with the page template ||
|#

### Object blocks of the page {#page-blocks}

#|
|| **Name**
`type` | **Description** ||
|| **`#block<id>`**
[`object`](../../data-types.md) | Export of a page block [(detailed description)](#block-item) ||
|#

### Object block {#block-item}

Fields of the block that do not contain data are removed from the result and may be absent in the object

#|
|| **Name**
`type` | **Description** ||
|| **old_id**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Original block identifier ||
|| **code**
[`string`](../../data-types.md) | Block code ||
|| **access**
[`string`](../../data-types.md) | Block access level ||
|| **anchor**
[`string`](../../data-types.md) | Local anchor of the block ||
|| **repo_block**
[`object`](../../data-types.md) | Block data from the repository [(detailed description)](#repo-block) ||
|| **cards**
[`object`](../../data-types.md) | Export of block cards ||
|| **nodes**
[`object`](../../data-types.md) | Export of block nodes ||
|| **menu**
[`object`](../../data-types.md) | Export of block menu ||
|| **style**
[`object`](../../data-types.md) | Export of block styles ||
|| **attrs**
[`object`](../../data-types.md) | Export of block attributes ||
|| **dynamic**
[`object`](../../data-types.md) | Export of block dynamic parameters ||
|#

### Object repo_block {#repo-block}

#|
|| **Name**
`type` | **Description** ||
|| **app_code**
[`string`](../../data-types.md) | Code of the source application of the block ||
|| **xml_id**
[`string`](../../data-types.md) | XML_ID of the block in the repository ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "SYSTEM_ERROR",
    "error_description": "The parameter code can only consist of Latin letters and digits."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `id` is missing ||
|| `ACCESS_DENIED` | Insufficient rights to access sites ||
|| `TYPE_ERROR` | Parameter of incorrect type was provided ||
|| `SYSTEM_ERROR` | Internal execution error, for example, `params.code` contains characters other than Latin letters and digits ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add.md)
- [{#T}](./landing-site-update.md)
- [{#T}](./landing-site-get-list.md)
- [{#T}](./landing-site-delete.md)
- [{#T}](../demos/landing-demos-register.md)