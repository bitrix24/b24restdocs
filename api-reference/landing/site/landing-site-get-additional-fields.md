# Get Additional Fields of the Site landing.site.getadditionalfields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" access permission for the site

The method `landing.site.getadditionalfields` retrieves additional fields of the site.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../data-types.md) | Internal scope of the landing pages. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../types.md) ||
|| **id*** 
[`integer`](../../data-types.md) | Identifier of the site.

The site identifier can be obtained using the method [landing.site.getList](./landing-site-get-list.md) or from the result of the method [landing.site.add](./landing-site-add.md) ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 205
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.getadditionalfields.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 205,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.getadditionalfields.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.getadditionalfields',
    		{
    			id: 205
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
                'landing.site.getadditionalfields',
                [
                    'id' => 205,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting additional fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getadditionalfields',
        {
            id: 205
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
        'landing.site.getadditionalfields',
        [
            'id' => 205,
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

Below is a shortened example of a response. The actual set of fields depends on the specific site settings.

```json
{
    "result": {
        "COOKIES_USE": "Y",
        "COOKIES_AGREEMENT_ID": "19",
        "COOKIES_COLOR_BG": "#03c1fe",
        "COOKIES_COLOR_TEXT": "#fff",
        "COOKIES_POSITION": "bottom_left",
        "COOKIES_MODE": "I",
        "B24BUTTON_CODE": "https://cdn.com.bitrix24.com/b13743910/crm/site_button/loader_1_68znkz.js",
        "B24BUTTON_COLOR": "site",
        "B24BUTTON_COLOR_VALUE": "#03c1fe",
        "COPYRIGHT_SHOW": "Y",
        "COPYRIGHT_CODE": "6",
        "SETTINGS_PRICE_CODE": [
            "BASE"
        ],
        "SETTINGS_SHOW_PRICE_COUNT": 1,
        "SETTINGS_CURRENCY_ID": "EUR",
        "SPEED_USE_LAZY": "Y",
        "THEME_CODE": "1construction",
        "THEME_COLOR": "#f7b70b",
        "YACOUNTER_USE": "Y",
        "YACOUNTER_COUNTER": "73521121",
        "ROBOTS_USE": "Y",
        "ROBOTS_CONTENT": "Disallow: /preview/*",
        "CSSBLOCK_USE": "N",
        "HEADBLOCK_USE": "N"
    },
    "time": {
        "start": 1773278929,
        "finish": 1773278929.806224,
        "duration": 0.8062241077423096,
        "processing": 0,
        "date_start": "2026-03-12T04:28:49+01:00",
        "date_finish": "2026-03-12T04:28:49+01:00",
        "operating_reset_at": 1773279529,
        "operating": 0.11928892135620117
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) \| `null` | A set of additional fields of the site in the format `{"<FIELD_CODE>": "<VALUE>"}`. 

If there are no available fields, the method may return `null` or an empty array `[]` [(detailed description)](#result-fields) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result-fields}

#| 
|| **Name**
`type` | **Description** ||
|| **<FIELD_CODE>**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) \| [`boolean`](../../data-types.md) \| [`array`](../../data-types.md) \| [`object`](../../data-types.md) | Pair "field code → field value". 

The value can be a string (`"Y"`, `"#03c1fe"`), a number (`1`), or an array (for example, `["BASE"]`). 

The composition and type of the value depend on the specific additional field. Available field codes are described in the [list of additional fields of the site](./additional-fields.md) ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough call parameters, missing: id"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `id` is missing ||
|| `ACCESS_DENIED` | Insufficient rights to call the method ||
|| `TYPE_ERROR` | Data type error in the method call parameters ||
|| `SYSTEM_ERROR` | Internal error while executing the method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add.md)
- [{#T}](./landing-site-update.md)
- [{#T}](./landing-site-get-folders.md)
- [{#T}](./landing-site-get-list.md)
- [{#T}](./landing-site-get-preview.md)
- [{#T}](./landing-site-get-public-url.md)