# Register a Template in the Site Creation Wizard landing.demos.register

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with View access permission in the Sites section

The method `landing.demos.register` registers a custom template in the site and page creation wizard.

The method updates the template if it already exists with the same code for the current application. If it does not exist, it creates a new one.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **data**^*^
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Template data.

Typically, the result of the method [landing.site.fullExport](../site/landing-site-full-export.md) is passed.

The method accepts both the site export with `items` and an array of individual template elements [more details](#data) ||
|| **params**
[`object`](../../data-types.md) | Additional registration parameters [more details](#params) ||
|#

### Type data {#data}

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **charset**
[`string`](../../data-types.md) | Encoding of the exported template ||
|| **code**^*^
[`string`](../../data-types.md) | External code of the template ||
|| **site_code**
[`string`](../../data-types.md) | Site code (path) ||
|| **name**
[`string`](../../data-types.md) | Template name ||
|| **description**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Template description ||
|| **type**
[`string`](../../data-types.md) | Template type.

Possible values:
- `page` — templates for pages
- `store` — templates for stores
- `knowledge` — templates for knowledge bases
- `group` — templates for groups
- `mainpage` — templates for main pages ||
|| **tpl_type**
[`string`](../../data-types.md) | Template usage type in the wizard.

Possible values:
- `S` — site template
- `P` — page template ||
|| **fields**
[`object`](../../data-types.md) | Site fields from the export [more details](../site/landing-site-full-export.md#result-fields) ||
|| **folders**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Folders from the export [more details](../site/landing-site-full-export.md#result) ||
|| **items**
[`object`](../../data-types.md) | Map of template pages in the format `{ "page_code": items }` [more details](#data-items-element) ||
|| **layout**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Layout data from the export [more details](../site/landing-site-full-export.md#result-layout) ||
|| **preview**
[`string`](../../data-types.md) | Link to preview 1x ||
|| **preview2x**
[`string`](../../data-types.md) | Link to preview 2x ||
|| **preview3x**
[`string`](../../data-types.md) | Link to preview 3x ||
|| **preview_url**
[`string`](../../data-types.md) | Link to preview ||
|| **show_in_list**
[`string`](../../data-types.md) | Indicator for display in the list (`Y`/`N`) ||
|| **syspages**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | System pages from the export [more details](../site/landing-site-full-export.md#result) ||
|| **version**
[`integer`](../../data-types.md) | Version of the export format ||
|#

### Type of data.items element {#data-items-element}

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **old_id**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Original page identifier ||
|| **code**^*^
[`string`](../../data-types.md) | External page code ||
|| **name**
[`string`](../../data-types.md) | Page name ||
|| **description**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Page description ||
|| **type**
[`string`](../../data-types.md) | Page type.

Possible values:
- `page` — templates for pages
- `store` — templates for stores
- `knowledge` — templates for knowledge bases
- `group` — templates for groups
- `mainpage` — templates for main pages ||
|| **tpl_type**
[`string`](../../data-types.md) | Template usage type in the wizard.

Possible values:
- `S` — site template
- `P` — page template ||
|| **version**
[`integer`](../../data-types.md) | Version of the page format ||
|| **fields**
[`object`](../../data-types.md) | Page fields from the export [more details](../site/landing-site-full-export.md#page-fields).

Codes for `fields.ADDITIONAL_FIELDS` can be found in the section [Additional Page Fields](../page/additional-fields.md) ||
|| **layout**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Page layout from the export [more details](../site/landing-site-full-export.md#page-layout) ||
|| **items**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Page blocks [more details](#data-items-items-element) ||
|| **preview**
[`string`](../../data-types.md) | Link to preview 1x ||
|| **preview2x**
[`string`](../../data-types.md) | Link to preview 2x ||
|| **preview3x**
[`string`](../../data-types.md) | Link to preview 3x ||
|| **preview_url**
[`string`](../../data-types.md) | Link to preview ||
|| **show_in_list**
[`string`](../../data-types.md) | Indicator for display in the list (`Y`/`N`) ||
|#

### Type of data.items.items element {#data-items-items-element}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../../data-types.md) | Block code ||
|| **access**
[`string`](../../data-types.md) | Access level to the block ||
|| **anchor**
[`string`](../../data-types.md) | Block anchor ||
|| **old_id**
[`integer`](../../data-types.md) | Original block identifier ||
|| **cards**
[`object`](../../data-types.md) | Block cards ||
|| **nodes**
[`object`](../../data-types.md) | Block nodes ||
|| **style**
[`object`](../../data-types.md) | Block styles ||
|| **attrs**
[`object`](../../data-types.md) | Block attributes ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **site_template_id**
[`string`](../../data-types.md) | Identifier of the site template of the main module.

Used in on-premise versions ||
|| **lang**
[`object`](../../data-types.md) | Localization of the main phrases of the template.

Details in the article [Template Localization](./localization.md) ||
|| **lang_original**
[`string`](../../data-types.md) | Code of the original language for the `lang` array ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of registering a template where:
- `data` — structure of the template for registration
- `data` in the examples is obtained in advance using the method [landing.site.fullExport](../site/landing-site-full-export.md)

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "data": {
          "charset": "UTF-8",
          "code": "ftmlt",
          "site_code": "/ftmlt/",
          "name": "Business",
          "description": null,
          "type": "page",
          "fields": {
            "TITLE": "Business",
            "LANDING_ID_INDEX": "0",
            "LANDING_ID_404": "0",
            "ADDITIONAL_FIELDS": {}
          },
          "folders": [],
          "items": {
            "ftmlt": {
              "old_id": "16",
              "code": "ftmlt",
              "name": "Business",
              "description": null,
              "preview": "",
              "preview2x": "",
              "preview3x": "",
              "preview_url": "",
              "show_in_list": "Y",
              "type": "page",
              "version": 3,
              "fields": {
                "TITLE": "Business"
              },
              "layout": [],
              "items": {}
            }
          },
          "layout": [],
          "preview": "",
          "preview2x": "",
          "preview3x": "",
          "preview_url": "",
          "show_in_list": "Y",
          "syspages": [],
          "version": 3
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.demos.register.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "data": {
          "charset": "UTF-8",
          "code": "ftmlt",
          "site_code": "/ftmlt/",
          "name": "Business",
          "description": null,
          "type": "page",
          "fields": {
            "TITLE": "Business",
            "LANDING_ID_INDEX": "0",
            "LANDING_ID_404": "0",
            "ADDITIONAL_FIELDS": {}
          },
          "folders": [],
          "items": {
            "ftmlt": {
              "old_id": "16",
              "code": "ftmlt",
              "name": "Business",
              "description": null,
              "preview": "",
              "preview2x": "",
              "preview3x": "",
              "preview_url": "",
              "show_in_list": "Y",
              "type": "page",
              "version": 3,
              "fields": {
                "TITLE": "Business"
              },
              "layout": [],
              "items": {}
            }
          },
          "layout": [],
          "preview": "",
          "preview2x": "",
          "preview3x": "",
          "preview_url": "",
          "show_in_list": "Y",
          "syspages": [],
          "version": 3
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.demos.register.json"
    ```

- JS

    ```js
    try
    {
    	const data = {
    		charset: 'UTF-8',
    		code: 'ftmlt',
    		site_code: '/ftmlt/',
    		name: 'Business',
    		description: null,
    		type: 'page',
    		fields: {
    			TITLE: 'Business',
    			LANDING_ID_INDEX: '0',
    			LANDING_ID_404: '0',
    			ADDITIONAL_FIELDS: {}
    		},
    		folders: [],
    		items: {
    			ftmlt: {
    				old_id: '16',
    				code: 'ftmlt',
    				name: 'Business',
    				description: null,
    				preview: '',
    				preview2x: '',
    				preview3x: '',
    				preview_url: '',
    				show_in_list: 'Y',
    				type: 'page',
    				version: 3,
    				fields: {
    					TITLE: 'Business'
    				},
    				layout: [],
    				items: {}
    			}
    		},
    		layout: [],
    		preview: '',
    		preview2x: '',
    		preview3x: '',
    		preview_url: '',
    		show_in_list: 'Y',
    		syspages: [],
    		version: 3
    	};

    	const response = await $b24.callMethod(
    		'landing.demos.register',
    		{
    			data
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
        $data = [
            'charset' => 'UTF-8',
            'code' => 'ftmlt',
            'site_code' => '/ftmlt/',
            'name' => 'Business',
            'description' => null,
            'type' => 'page',
            'fields' => [
                'TITLE' => 'Business',
                'LANDING_ID_INDEX' => '0',
                'LANDING_ID_404' => '0',
                'ADDITIONAL_FIELDS' => [],
            ],
            'folders' => [],
            'items' => [
                'ftmlt' => [
                    'old_id' => '16',
                    'code' => 'ftmlt',
                    'name' => 'Business',
                    'description' => null,
                    'preview' => '',
                    'preview2x' => '',
                    'preview3x' => '',
                    'preview_url' => '',
                    'show_in_list' => 'Y',
                    'type' => 'page',
                    'version' => 3,
                    'fields' => [
                        'TITLE' => 'Business',
                    ],
                    'layout' => [],
                    'items' => [],
                ],
            ],
            'layout' => [],
            'preview' => '',
            'preview2x' => '',
            'preview3x' => '',
            'preview_url' => '',
            'show_in_list' => 'Y',
            'syspages' => [],
            'version' => 3,
        ];

        $response = $b24Service
            ->core
            ->call(
                'landing.demos.register',
                [
                    'data' => $data,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error registering demo: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.demos.register',
        {
            data: {
                charset: 'UTF-8',
                code: 'ftmlt',
                site_code: '/ftmlt/',
                name: 'Business',
                description: null,
                type: 'page',
                fields: {
                    TITLE: 'Business',
                    LANDING_ID_INDEX: '0',
                    LANDING_ID_404: '0',
                    ADDITIONAL_FIELDS: {}
                },
                folders: [],
                items: {
                    ftmlt: {
                        old_id: '16',
                        code: 'ftmlt',
                        name: 'Business',
                        description: null,
                        preview: '',
                        preview2x: '',
                        preview3x: '',
                        preview_url: '',
                        show_in_list: 'Y',
                        type: 'page',
                        version: 3,
                        fields: {
                            TITLE: 'Business'
                        },
                        layout: [],
                        items: {}
                    }
                },
                layout: [],
                preview: '',
                preview2x: '',
                preview3x: '',
                preview_url: '',
                show_in_list: 'Y',
                syspages: [],
                version: 3
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

    $data = [
        'charset' => 'UTF-8',
        'code' => 'ftmlt',
        'site_code' => '/ftmlt/',
        'name' => 'Business',
        'description' => null,
        'type' => 'page',
        'fields' => [
            'TITLE' => 'Business',
            'LANDING_ID_INDEX' => '0',
            'LANDING_ID_404' => '0',
            'ADDITIONAL_FIELDS' => [],
        ],
        'folders' => [],
        'items' => [
            'ftmlt' => [
                'old_id' => '16',
                'code' => 'ftmlt',
                'name' => 'Business',
                'description' => null,
                'preview' => '',
                'preview2x' => '',
                'preview3x' => '',
                'preview_url' => '',
                'show_in_list' => 'Y',
                'type' => 'page',
                'version' => 3,
                'fields' => [
                    'TITLE' => 'Business',
                ],
                'layout' => [],
                'items' => [],
            ],
        ],
        'layout' => [],
        'preview' => '',
        'preview2x' => '',
        'preview3x' => '',
        'preview_url' => '',
        'show_in_list' => 'Y',
        'syspages' => [],
        'version' => 3,
    ];

    $result = CRest::call(
        'landing.demos.register',
        [
            'data' => $data,
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
    "result": [5, 7],
    "time": {
        "start": 1774611129,
        "finish": 1774611129.843163,
        "duration": 0.843163013458252,
        "processing": 0,
        "date_start": "2026-03-27T14:32:09+02:00",
        "date_finish": "2026-03-27T14:32:09+02:00",
        "operating_reset_at": 1774611729,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer[]`](../../data-types.md) | Array of identifiers of the templates that were created or updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "The value of an argument 'data' has an invalid type",
    "argument": "data"
}
```

```json
{
    "error": "REGISTER_ERROR_DATA",
    "error_description": "Data is empty or incorrect"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | The value of an argument 'data' has an invalid type | Parameter `data` is passed in an incorrect type ||
|| `REGISTER_ERROR_DATA` | Data is empty or incorrect | Parameter `data` is empty or invalid ||
|| `CONTENT_IS_BAD` | Content is defined as unsafe. Unsafe parts can be identified using the method `landing.repo.checkcontent` | Unsafe content found in the provided template ||
|| `BX_EMPTY_REQUIRED` | Required field "External code" is not filled | `data` does not have `code` (external code) for the template/element ||
|| `ACCESS_DENIED` | `Access denied!` | Insufficient permissions to call the method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-demos-get-site-list.md)
- [{#T}](./landing-demos-get-page-list.md)
- [{#T}](./landing-demos-get-list.md)
- [{#T}](./landing-demos-unregister.md)
- [{#T}](./localization.md)
- [{#T}](./index.md)