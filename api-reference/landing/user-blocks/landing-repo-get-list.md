# Get a List of Custom Blocks landing.repo.getList

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with View access permission in the Sites section

The method `landing.repo.getList` retrieves a list of custom blocks.

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

For more details on each parameter, see the [params parameter](#params) section ||
|#

### params Parameter {#params}

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

For a list of available fields for selection, see the [result element type](#result-template) section.

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

For a list of available fields for filtering, see the [result element type](#result-template) section.

If `filter` is not provided or is not in the `object` format, the method uses an empty filter `{}` ||
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

For a list of available fields for sorting, see the [result element type](#result-template) section ||
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
- `["ACTIVE"]`
- `["APP_CODE", "ACTIVE"]`

For a list of available fields, see the [result element type](#result-template) section ||
|| **limit**
[`integer`](../../data-types.md) | Limit of records ||
|| **offset**
[`integer`](../../data-types.md) | Offset of records ||
|#

{% note info %}

If the method is called in the context of an application, the server additionally adds a filter for the current application.

In this case, only blocks created by the same application will be included in the response.

{% endnote %}


## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of retrieving a list of blocks, where:
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
          "select": ["ID", "NAME", "DATE_MODIFY"],
          "filter": {"ACTIVE": "Y"},
          "order": {"ID": "DESC"},
          "group": ["ACTIVE"]
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.repo.getList.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "params": {
          "select": ["ID", "NAME", "DATE_MODIFY"],
          "filter": {"ACTIVE": "Y"},
          "order": {"ID": "DESC"},
          "group": ["ACTIVE"]
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.repo.getList.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.repo.getList',
    		{
    			params: {
    				select: ['ID', 'NAME', 'DATE_MODIFY'],
    				filter: { ACTIVE: 'Y' },
    				order: { ID: 'DESC' },
    				group: ['ACTIVE']
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
                'landing.repo.getList',
                [
                    'params' => [
                        'select' => ['ID', 'NAME', 'DATE_MODIFY'],
                        'filter' => ['ACTIVE' => 'Y'],
                        'order' => ['ID' => 'DESC'],
                        'group' => ['ACTIVE'],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting landing repository list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repo.getList',
        {
            params: {
                select: ['ID', 'NAME', 'DATE_MODIFY'],
                filter: { ACTIVE: 'Y' },
                order: { ID: 'DESC' },
                group: ['ACTIVE']
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
        'landing.repo.getList',
        [
            'params' => [
                'select' => ['ID', 'NAME', 'DATE_MODIFY'],
                'filter' => ['ACTIVE' => 'Y'],
                'order' => ['ID' => 'DESC'],
                'group' => ['ACTIVE'],
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
    "result": [
        {
            "ID": "5",
            "XML_ID": "ctx_full_1774873150158",
            "APP_CODE": "bitrix.restapi",
            "ACTIVE": "Y",
            "NAME": "Context full test block",
            "DESCRIPTION": "Check full fields from getList",
            "SECTIONS": "cover,about",
            "SITE_TEMPLATE_ID": null,
            "PREVIEW": "https://www.bitrix24.com/images/b24_screen.png",
            "MANIFEST": {
                "block": {
                    "name": "Context full test block"
                },
                "nodes": {
                    ".landing-block-node-text": {
                        "name": "Text",
                        "type": "text"
                    }
                }
            },
            "CONTENT": "<section class=\"landing-block\"><div class=\"container\">Test</div></section>",
            "CREATED_BY_ID": "577",
            "MODIFIED_BY_ID": "577",
            "DATE_CREATE": "30.03.2026 14:19:11",
            "DATE_MODIFY": "30.03.2026 14:19:11"
        }
    ],
    "time": {
        "start": 1774873153,
        "finish": 1774873153.634216,
        "duration": 0.6342160701751709,
        "processing": 0,
        "date_start": "2026-03-30T15:19:13+02:00",
        "date_finish": "2026-03-30T15:19:13+02:00",
        "operating_reset_at": 1774873753,
        "operating": 0.11733078956604004
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../data-types.md) | List of blocks [more details](#result-template) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Result Element Type {#result-template}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Identifier of the block ||
|| **XML_ID**
[`string`](../../data-types.md) | External code of the block ||
|| **APP_CODE**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Application code ||
|| **ACTIVE**
[`string`](../../data-types.md) | Activity status (`Y`/`N`) ||
|| **NAME**
[`string`](../../data-types.md) | Name of the block ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the block ||
|| **SECTIONS**
[`string`](../../data-types.md) | Sections of the block ||
|| **SITE_TEMPLATE_ID**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the site template ||
|| **PREVIEW**
[`string`](../../data-types.md) | Link to preview ||
|| **MANIFEST**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) \| [`boolean`](../../data-types.md) | Manifest of the block.

For more details on the manifest structure: [Manifest format description](../block/manifest.md).

Example of a manifest in the method response: [landing.block.getManifestFile](../block/methods/landing-block-get-manifest-file.md) ||
|| **CONTENT**
[`string`](../../data-types.md) | HTML of the block ||
|| **CREATED_BY_ID**
[`string`](../../data-types.md) | Identifier of the author ||
|| **MODIFIED_BY_ID**
[`string`](../../data-types.md) | Identifier of the user who modified the record ||
|| **DATE_CREATE**
[`string`](../../data-types.md) | Creation date in the format `DD.MM.YYYY HH:MI:SS` ||
|| **DATE_MODIFY**
[`string`](../../data-types.md) | Modification date in the format `DD.MM.YYYY HH:MI:SS` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "The value of an argument 'params' has an invalid type",
    "argument": "params"
}
```

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Insufficient permissions."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | Insufficient permissions | User did not pass general access checks ||
|| `ERROR_ARGUMENT` | The value of an argument 'params' has an invalid type | The `params` argument was passed in an incorrect type ||
|| `SYSTEM_ERROR` | Internal error | Error executing the method on the server side ||
|| `insufficient_scope` | Insufficient scope for the token | The token does not contain the `landing` scope ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-repo-register.md)
- [{#T}](./landing-repo-check-content.md)
- [{#T}](./landing-repo-unregister.md)
- [{#T}](./index.md)