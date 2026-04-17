# Get Additional Fields of the Page landing.landing.getadditionalfields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" access permission for the site

The method `landing.landing.getadditionalfields` retrieves [additional fields](../additional-fields.md) of the page.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of the landing. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid***
[`integer`](../../../data-types.md) | Identifier of the page.

The page identifier can be obtained using the [landing.landing.getList](./landing-landing-get-list.md) method, as well as from the results of the [landing.landing.add](./landing-landing-add.md), [landing.landing.addByTemplate](./landing-landing-add-by-template.md), and [landing.landing.copy](./landing-landing-copy.md) methods ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 349
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.getadditionalfields.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 349,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.getadditionalfields.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.landing.getadditionalfields',
            {
                lid: 349
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
                'landing.landing.getadditionalfields',
                [
                    'lid' => 349,
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
        'landing.landing.getadditionalfields',
        {
            lid: 349
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
        'landing.landing.getadditionalfields',
        [
            'lid' => 349,
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

Below is a shortened example of a response. The actual set of fields depends on the page settings and the additional fields connected to it.

```json
{
    "result": {
        "FONTS_CODE": "<noscript><link rel=\"stylesheet\" href=\"https://fonts.googleapis.com/...\" data-font=\"g-font-russo-one\"></noscript>",
        "GACOUNTER_USE": "N",
        "METAMAIN_USE": "Y",
        "METAMAIN_TITLE": "Festival in New York. April 20-26, 2022. Buy tickets online",
        "METAOG_TITLE": "Festival in New York. April 20-26, 2022. Buy tickets online",
        "METAOG_IMAGE": "https://cdn.com/.../cover_1x.webp",
        "SETTINGS_PRICE_CODE": [
            "BASE"
        ],
        "SETTINGS_SHOW_PRICE_COUNT": 1,
        "THEMEFONTS_LINE_HEIGHT": "1.6",
        "VIEW_TYPE": "no",
        "YACOUNTER_USE": "N"
    },
    "time": {
        "start": 1773722096,
        "finish": 1773722096.682451,
        "duration": 0.6824510097503662,
        "processing": 0,
        "date_start": "2026-03-17T12:34:56+01:00",
        "date_finish": "2026-03-17T12:34:56+01:00",
        "operating_reset_at": 1773722696,
        "operating": 0.11843705177307129
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | A set of additional fields of the page in the format `{"<FIELD_CODE>": "<VALUE>"}`.

If the page has no available non-empty additional fields, the method returns an empty array `[]` [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **<FIELD_CODE>**
[`string`](../../../data-types.md) \| [`integer`](../../../data-types.md) \| [`boolean`](../../../data-types.md) \| [`array`](../../../data-types.md) \| [`object`](../../../data-types.md) | Pair "field code → field value". The method returns only fields with non-empty values.  

Available field codes are listed in the section [Additional Fields of the Page](../additional-fields.md) ||
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
|| `MISSING_PARAMS` | Required parameter `lid` is missing ||
|| `LANDING_NOT_EXIST` | Page not found: the `lid` parameter contains the identifier of a non-existent or inaccessible page ||
|| `ACCESS_DENIED` | Insufficient rights to call the method ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add.md)
- [{#T}](./landing-landing-add-by-template.md)
- [{#T}](./landing-landing-copy.md)
- [{#T}](./landing-landing-update.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-get-preview.md)
- [{#T}](./landing-landing-get-public-url.md)