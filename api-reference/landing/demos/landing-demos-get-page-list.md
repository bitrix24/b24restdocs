# Get a List of Templates for Creating Pages landing.demos.getPageList

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with View access permission in the Sites section

The method `landing.demos.getPageList` retrieves a list of file demo templates for pages.

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

You can filter by fields from the [Page Template Type](#page-template) section. Nested fields (e.g., `DATA.*`) are not considered in the filter ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of retrieving a list of page templates, where:
- `type` — code of the template set
- `filter` — filter by template fields

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "type": "page",
        "filter": {
          "TYPE": "PAGE"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.demos.getPageList.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "type": "page",
        "filter": {
          "TYPE": "PAGE"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.demos.getPageList.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.demos.getPageList',
    		{
    			type: 'page',
    			filter: {
    				TYPE: 'PAGE'
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
                'landing.demos.getPageList',
                [
                    'type' => 'page',
                    'filter' => [
                        'TYPE' => 'PAGE',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting page demo list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.demos.getPageList',
        {
            type: 'page',
            filter: {
                TYPE: 'PAGE'
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
        'landing.demos.getPageList',
        [
            'type' => 'page',
            'filter' => [
                'TYPE' => 'PAGE',
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
            "TYPE": ["PAGE", "KNOWLEDGE", "GROUP", "MAINPAGE"],
            "TITLE": "Empty Page",
            "ACTIVE": true,
            "PUBLICATION": false,
            "LOCK_DELETE": false,
            "AVAILABLE": true,
            "SINGLETON": false,
            "SECTION": [],
            "DESCRIPTION": "Create your own website from scratch and attract customers!",
            "PREVIEW": "//bitrix24.com/bitrix/components/bitrix/landing.demo/data/page/empty/preview.jpg",
            "PREVIEW2X": "//bitrix24.com/bitrix/components/bitrix/landing.demo/data/page/empty/preview@2x.jpg",
            "PREVIEW3X": "//bitrix24.com/bitrix/components/bitrix/landing.demo/data/page/empty/preview@3x.jpg",
            "APP_CODE": "",
            "REST": 0,
            "DATA": {
                "name": "Empty Page",
                "active": true,
                "type": ["PAGE", "KNOWLEDGE", "GROUP", "MAINPAGE"],
                "items": [],
                "version": 1,
                "old_id": 402,
                "encoded": true,
                "charset": "UTF-8"
            }
        },
        "search-result": {
            "ID": "search-result",
            "XML_ID": "search-result",
            "TYPE": ["PAGE", "KNOWLEDGE", "GROUP"],
            "TITLE": "Search Results",
            "ACTIVE": false,
            "PUBLICATION": true,
            "SECTION": ["dynamic"],
            "DATA": {
                "code": "search-result",
                "section": ["dynamic"],
                "publication": true,
                "layout": [],
                "items": {
                    "#block3430": {
                        "code": "59.1.search"
                    }
                },
                "encoded": true,
                "charset": "UTF-8"
            }
        }
    },
    "time": {
        "start": 1774625365,
        "finish": 1774625365.92986,
        "duration": 0.9298601150512695,
        "processing": 0,
        "date_start": "2026-03-27T18:29:25+02:00",
        "date_finish": "2026-03-27T18:29:25+02:00",
        "operating_reset_at": 1774625965,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Map of page demo templates in the format:

```
{
    "template_code": page_template
}
```

where:
- `template_code` — template code
- `page_template` — page template object [more details](#page-template)

If no suitable templates are found, the method returns an empty array `result: []` without an error.

The composition of template fields may vary and depends on the specific template ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

### Page Template Type {#page-template}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Template identifier ||
|| **XML_ID**
[`string`](../../data-types.md) | External template code ||
|| **TYPE**
[`string[]`](../../data-types.md) \| [`string`](../../data-types.md) | Types for which the template is available.

For file templates, an array is usually returned; for registered application templates — a string ||
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
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Template data from file source [more details](#page-template-data).

For registered application templates, an empty array may be returned ||
|#

### Template DATA Structure {#page-template-data}

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
|| **publication**
[`boolean`](../../data-types.md) | Template publication status in export data ||
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
[`integer`](../../data-types.md) | Template sorting index ||
|| **section**
[`string[]`](../../data-types.md) | Template sections in export data ||
|| **parent**
[`string`](../../data-types.md) | Parent template/site code ||
|| **disable_import**
[`string`](../../data-types.md) | Import prohibition flag in template data (e.g., `Y`) ||
|| **is_webform_page**
[`string`](../../data-types.md) | CRM form page flag (e.g., `Y`) ||
|| **fields**
[`object`](../../data-types.md) | Template fields [more details](../site/landing-site-full-export.md#page-fields).

Codes for `fields.ADDITIONAL_FIELDS` can be found in the [Additional Page Fields](../page/additional-fields.md) section ||
|| **items**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Template composition [more details](../site/landing-site-full-export.md#page-blocks) ||
|| **layout**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Template layout data [more details](../site/landing-site-full-export.md#page-layout) ||
|| **folders**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Template folders [more details](../site/landing-site-full-export.md#result) ||
|| **syspages**
[`array`](../../data-types.md) \| [`object`](../../data-types.md) | Template system pages [more details](../site/landing-site-full-export.md#result) ||
|| **master_pages**
[`array`](../../data-types.md) | List of template master pages [more details](../site/landing-site-full-export.md#result) ||
|| **version**
[`integer`](../../data-types.md) | Data format version ||
|| **old_id**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Original identifier (may come as a number or string depending on the template) ||
|| **encoded**
[`boolean`](../../data-types.md) | Added by the method with a value of `true` if the `DATA` contains the `items` field ||
|| **charset**
[`string`](../../data-types.md) | Added by the method with a value of `UTF-8` if the `DATA` contains the `items` field ||
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
- [{#T}](./landing-demos-get-site-list.md)
- [{#T}](./landing-demos-get-list.md)
- [{#T}](./landing-demos-unregister.md)
- [{#T}](./localization.md)
- [{#T}](./index.md)