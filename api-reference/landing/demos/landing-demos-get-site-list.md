# Get a List of Templates for Creating Websites landing.demos.getSiteList

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with View access permission in the Sites section

The method `landing.demos.getSiteList` retrieves a list of file demo templates for websites.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type**^*^
[`string`](../../data-types.md) | Template type.

Possible values:
- `page` — templates for pages
- `store` — templates for stores
- `knowledge` — templates for knowledge bases
- `group` — templates for groups
- `mainpage` — templates for main pages ||
|| **filter**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — filter field
- `value_n` — filter value

Filtering is applied to the top-level fields of the template object.

You can filter by fields from the [Website Template Type](#site-template). Nested fields (e.g., `DATA.*`) are not considered in the filter ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of retrieving a list of website templates, where:
- `type` — code of the template set
- `filter` — filter by website template fields

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "type": "page",
        "filter": {
          "TYPE": "page"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.demos.getSiteList.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "type": "page",
        "filter": {
          "TYPE": "page"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.demos.getSiteList.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.demos.getSiteList',
    		{
    			type: 'page',
    			filter: {
    				TYPE: 'page'
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
                'landing.demos.getSiteList',
                [
                    'type' => 'page',
                    'filter' => [
                        'TYPE' => 'page',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting site demo list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.demos.getSiteList',
        {
            type: 'page',
            filter: {
                TYPE: 'page'
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
        'landing.demos.getSiteList',
        [
            'type' => 'page',
            'filter' => [
                'TYPE' => 'page',
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
    "result": {
        "empty": {
            "ID": "empty",
            "XML_ID": "empty",
            "TYPE": ["KNOWLEDGE", "GROUP", "PAGE", "MAINPAGE"],
            "TITLE": "Empty Template",
            "ACTIVE": true,
            "PUBLICATION": false,
            "LOCK_DELETE": false,
            "AVAILABLE": true,
            "SINGLETON": false,
            "SECTION": [],
            "DESCRIPTION": "Create your own website from scratch and attract customers!",
            "PREVIEW": "//bitrix24.com/bitrix/components/bitrix/landing.demo/data/site/empty/preview.jpg",
            "PREVIEW2X": "//bitrix24.com/bitrix/components/bitrix/landing.demo/data/site/empty/preview@2x.jpg",
            "PREVIEW3X": "//bitrix24.com/bitrix/components/bitrix/landing.demo/data/site/empty/preview@3x.jpg",
            "APP_CODE": "",
            "REST": 0,
            "DATA": {
                "name": "Empty Template",
                "items": [],
                "encoded": true,
                "charset": "UTF-8"
            }
        }
    },
    "time": {
        "start": 1774623040,
        "finish": 1774623040.337361,
        "duration": 0.33736109733581543,
        "processing": 0,
        "date_start": "2026-03-27T17:50:40+01:00",
        "date_finish": "2026-03-27T17:50:40+01:00",
        "operating_reset_at": 1774623640,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Map of website demo templates in the format:

```
{
    "template_code": site_template
}
```

where:
- `template_code` — template code
- `site_template` — template object [more details](#site-template)

If no suitable templates are found, the method returns an empty array `result: []`.

The composition of template fields may vary and depends on the specific template ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Website Template Type {#site-template}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Template identifier ||
|| **XML_ID**
[`string`](../../data-types.md) | External template code ||
|| **TYPE**
[`string[]`](../../data-types.md) \| [`string`](../../data-types.md) | Types for which the template is available.

For file templates, an array is usually returned; for registered application templates, a string ||
|| **TITLE**
[`string`](../../data-types.md) | Template title ||
|| **ACTIVE**
[`boolean`](../../data-types.md) | Active status ||
|| **PUBLICATION**
[`boolean`](../../data-types.md) | Publication availability status ||
|| **LOCK_DELETE**
[`boolean`](../../data-types.md) | Deletion prohibition status ||
|| **AVAILABLE**
[`boolean`](../../data-types.md) | Template availability status ||
|| **SINGLETON**
[`boolean`](../../data-types.md) | Singleton status ||
|| **SECTION**
[`string[]`](../../data-types.md) | Template sections ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Template description ||
|| **PREVIEW**
[`string`](../../data-types.md) | Preview 1x ||
|| **PREVIEW2X**
[`string`](../../data-types.md) | Preview 2x ||
|| **PREVIEW3X**
[`string`](../../data-types.md) | Preview 3x ||
|| **APP_CODE**
[`string`](../../data-types.md) | Application code ||
|| **REST**
[`integer`](../../data-types.md) | REST template status ||
|| **DATA**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Template data from file source [more details](#site-template-data).

For registered application templates, an empty array may be returned ||
|#

### Template DATA Type {#site-template-data}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../../data-types.md) | Template code in export data ||
|| **name**
[`string`](../../data-types.md) | Template name in export data ||
|| **type**
[`string[]`](../../data-types.md) | Template types.

Possible values:
- `page` — templates for pages
- `store` — templates for stores
- `knowledge` — templates for knowledge bases
- `group` — templates for groups
- `mainpage` — templates for main pages ||
|| **description**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Template description ||
|| **active**
[`boolean`](../../data-types.md) | Template active status in export data ||
|| **singleton**
[`boolean`](../../data-types.md) | Template singleton status in export data ||
|| **lock_delete**
[`boolean`](../../data-types.md) | Deletion prohibition status in export data ||
|| **preview**
[`string`](../../data-types.md) | Preview 1x in export data ||
|| **preview2x**
[`string`](../../data-types.md) | Preview 2x in export data ||
|| **preview3x**
[`string`](../../data-types.md) | Preview 3x in export data ||
|| **preview_url**
[`string`](../../data-types.md) | Preview link in export data ||
|| **show_in_list**
[`string`](../../data-types.md) | Show in list status (`Y`/`N`) ||
|| **sort**
[`integer`](../../data-types.md) | Template sort index ||
|| **fields**
[`object`](../../data-types.md) | Template fields [more details](../site/landing-site-full-export.md#result-fields).

Codes for `fields.ADDITIONAL_FIELDS` can be found in the section [Additional Website Fields](../site/additional-fields.md) ||
|| **items**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Template composition [more details](../site/landing-site-full-export.md#result-items) ||
|| **layout**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Template layout data [more details](../site/landing-site-full-export.md#result-layout) ||
|| **folders**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Template folders [more details](../site/landing-site-full-export.md#result) ||
|| **syspages**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Template system pages [more details](../site/landing-site-full-export.md#result) ||
|| **master_pages**
[`array`](../../data-types.md) | List of template master pages [more details](../site/landing-site-full-export.md#result) ||
|| **version**
[`integer`](../../data-types.md) | Data format version ||
|| **old_id**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Original identifier ||
|| **encoded**
[`boolean`](../../data-types.md) | Added by the method with the value `true` if there is a field `items` in `DATA` ||
|| **charset**
[`string`](../../data-types.md) | Added by the method with the value `UTF-8` if there is a field `items` in `DATA` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "The value of an argument 'filter' has an invalid type",
    "argument": "filter"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | The value of an argument 'filter' has an invalid type | The `filter` parameter is passed in an incorrect type ||
|| `MISSING_PARAMS` | Insufficient call parameters | The required parameter `type` is missing ||
|| `ACCESS_DENIED` | Insufficient permissions | The user did not pass the general access checks for the Landing module ||
|| `TYPE_ERROR` | Data type error | Method call with incorrect parameter types ||
|| `SYSTEM_ERROR` | Internal error | Error executing the method on the server side ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-demos-register.md)
- [{#T}](./landing-demos-get-page-list.md)
- [{#T}](./landing-demos-get-list.md)
- [{#T}](./landing-demos-unregister.md)
- [{#T}](./localization.md)
- [{#T}](./index.md)