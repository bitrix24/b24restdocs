# Get a List of Registered Templates landing.demos.getList

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with View access permission in the Sites section

The method `landing.demos.getList` retrieves a list of registered templates.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **params**
[`object`](../../data-types.md) | Object format:

```
{
    select: value_1,
    filter: value_2,
    order: value_3,
    group: value_4,
    limit: value_5,
    offset: value_6
}
```

where:
- `value_n` — value of the corresponding selection parameter

For more details on each parameter, see the section [Parameter params](#params) ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | Array format:

```
[
    field_1,
    field_2,
    ...,
    field_n
]
```

where:
- `field_n` — selection field

For a list of available fields for selection, see the section [Result element type](#result-template).

Elements in `select` with a `.` are ignored ||
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
- `field_n` — filtering field
- `value_n` — filter value

For a list of available fields for filtering, see the section [Result element type](#result-template).

If `filter` is not provided or is not in `object` format, the method uses an empty filter `{}` ||
|| **order**
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
- `field_n` — sorting field
- `value_n` — sorting direction: `ASC` or `DESC`

For a list of available fields for sorting, see the section [Result element type](#result-template) ||
|| **group**
[`array`](../../data-types.md) | Array of fields for grouping the result.

Format:

```
[
    field_1,
    field_2,
    ...,
    field_n
]
```

where:
- `field_n` — grouping field

Examples:
- `["TYPE"]`
- `["ACTIVE", "TYPE"]`

For a list of available fields, see the section [Result element type](#result-template) ||
|| **limit**
[`integer`](../../data-types.md) | Limit on the number of records in the selection ||
|| **offset**
[`integer`](../../data-types.md) | Offset of records in the selection ||
|#

{% note info %}

If the method is called in the context of an application, the server additionally adds a filter for the current application.

In this case, only templates created by the same application will be included in the response.

When called outside the application context, this filter is not added.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of retrieving a list of templates, where:
- `params.select` — fields to return in the response
- `params.filter` — conditions for filtering records
- `params.order` — sorting the result
- `params.group` — grouping fields

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "params": {
          "select": ["ID", "XML_ID", "TITLE", "TYPE", "DATE_MODIFY"],
          "filter": {"ACTIVE": "Y"},
          "order": {"ID": "DESC"},
          "group": ["TYPE"]
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.demos.getList.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "params": {
          "select": ["ID", "XML_ID", "TITLE", "TYPE", "DATE_MODIFY"],
          "filter": {"ACTIVE": "Y"},
          "order": {"ID": "DESC"},
          "group": ["TYPE"]
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.demos.getList.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.demos.getList',
    		{
    			params: {
    				select: ['ID', 'XML_ID', 'TITLE', 'TYPE', 'DATE_MODIFY'],
    				filter: { ACTIVE: 'Y' },
    				order: { ID: 'DESC' },
    				group: ['TYPE']
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
                'landing.demos.getList',
                [
                    'params' => [
                        'select' => ['ID', 'XML_ID', 'TITLE', 'TYPE', 'DATE_MODIFY'],
                        'filter' => ['ACTIVE' => 'Y'],
                        'order' => ['ID' => 'DESC'],
                        'group' => ['TYPE'],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting demo list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.demos.getList',
        {
            params: {
                select: ['ID', 'XML_ID', 'TITLE', 'TYPE', 'DATE_MODIFY'],
                filter: { ACTIVE: 'Y' },
                order: { ID: 'DESC' },
                group: ['TYPE']
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
        'landing.demos.getList',
        [
            'params' => [
                'select' => ['ID', 'XML_ID', 'TITLE', 'TYPE', 'DATE_MODIFY'],
                'filter' => ['ACTIVE' => 'Y'],
                'order' => ['ID' => 'DESC'],
                'group' => ['TYPE'],
            ],
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "9",
            "XML_ID": "ftmlt/business",
            "TITLE": "Business",
            "TYPE": "page",
            "DATE_MODIFY": "03/27/2026 14:18:15",
            "MANIFEST": false
        }
    ],
    "time": {
        "start": 1774621455,
        "finish": 1774621455.226454,
        "duration": 0.2264540195465088,
        "processing": 0,
        "date_start": "2026-03-27T17:24:15+01:00",
        "date_finish": "2026-03-27T17:24:15+01:00",
        "operating_reset_at": 1774622055,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../data-types.md) | List of registered templates [more details](#result-template).

If no matching records are found, the method returns `result: []` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

### Result Element Type {#result-template}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the template ||
|| **XML_ID**
[`string`](../../data-types.md) | External code of the template ||
|| **APP_CODE**
[`string`](../../data-types.md) | Application code ||
|| **ACTIVE**
[`string`](../../data-types.md) | Activity status (`Y`/`N`) ||
|| **TITLE**
[`string`](../../data-types.md) | Title of the template ||
|| **DESCRIPTION**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Description of the template ||
|| **TYPE**
[`string`](../../data-types.md) | Type of the template.

Possible values:
- `page` — templates for pages/sites
- `store` — templates for stores
- `knowledge` — templates for knowledge bases
- `group` — templates for groups
- `mainpage` — templates for main pages ||
|| **TPL_TYPE**
[`string`](../../data-types.md) | Type of the wizard.

Possible values:
- `S` — site template
- `P` — page template ||
|| **SHOW_IN_LIST**
[`string`](../../data-types.md) | Indicator of display in the list (`Y`/`N`) ||
|| **PREVIEW_URL**
[`string`](../../data-types.md) | Link to preview ||
|| **PREVIEW**
[`string`](../../data-types.md) | Preview 1x ||
|| **PREVIEW2X**
[`string`](../../data-types.md) | Preview 2x ||
|| **PREVIEW3X**
[`string`](../../data-types.md) | Preview 3x ||
|| **MANIFEST**
[`object`](../../data-types.md) \| [`boolean`](../../data-types.md) | Manifest of the template.

In the original data, it is stored serialized, in the method response it is returned as an object after `unserialize`.

If the manifest is not specified, it may return `false` ||
|| **LANG**
[`object`](../../data-types.md) \| [`null`](../../data-types.md) | Localization of the template.

If the field is filled, it is returned in the response after `unserialize`.

The composition and keys of localization depend on the template. More details: [Template Localization](./localization.md) ||
|| **SITE_TEMPLATE_ID**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the site template of the main module ||
|| **CREATED_BY_ID**
[`integer`](../../data-types.md) | Identifier of the user who created the template ||
|| **MODIFIED_BY_ID**
[`integer`](../../data-types.md) | Identifier of the user who modified the template ||
|| **DATE_CREATE**
[`string`](../../data-types.md) | Creation date.

The method converts the value to a string ||
|| **DATE_MODIFY**
[`string`](../../data-types.md) | Modification date.

The method converts the value to a string ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "The value of an argument 'params' has an invalid type",
    "argument": "params"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | The value of an argument 'params' has an invalid type | The parameter `params` is passed in an incorrect type ||
|| `ACCESS_DENIED` | Insufficient permissions | The user did not pass general access checks ||
|| `TYPE_ERROR` | Data type error | Method call with incorrect parameter types ||
|| `SYSTEM_ERROR` | Internal error | Error executing the method on the server side ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-demos-register.md)
- [{#T}](./landing-demos-get-site-list.md)
- [{#T}](./landing-demos-get-page-list.md)
- [{#T}](./landing-demos-unregister.md)
- [{#T}](./localization.md)
- [{#T}](./index.md)