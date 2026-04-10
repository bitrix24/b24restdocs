# Set Included Areas for the Site landing.template.setSiteRef

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify settings" access permission for the site

The method `landing.template.setSiteRef` saves the bindings of included areas for the site. It only works with site bindings and does not change the bindings of individual pages.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Site identifier.

The site identifier can be obtained using the [landing.site.getList](../site/landing-site-get-list.md) method. ||
|| **data**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | A set of bindings for the included areas of the site [(detailed description)](#data).

If the parameter is not provided or an empty object `{}` or array `[]` is passed, the method will remove all current bindings of included areas for this site. ||
|#

### Parameter data {#data}

#|
|| **Name**
`type` | **Description** ||
|| **<AREA_ID>**
[`integer`](../../data-types.md) | Identifier of the page to be assigned to the corresponding included area.

The key is the identifier of the template area, and the value is the page identifier.

Area identifiers depend on the site template. They can be determined by the template, for example, through the [landing.template.getlist](./landing-template-get-list.md) method and the `CONTENT` field: in the markup, areas are denoted as `#AREA_1#`, `#AREA_2#`, and so on.

In `data`, only the numeric part of such an identifier should be passed: `1`, `2`. If a key is not provided for a saved area in `data` or if a value of `0`, an empty string, `null`, or a negative number is passed, the binding will be removed. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 157,
        "data": {
          "1": 614,
          "2": 615,
          "3": 616
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.template.setSiteRef.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 157,
        "data": {
          "1": 614,
          "2": 615,
          "3": 616
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.template.setSiteRef.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.template.setSiteRef',
            {
                id: 157,
                data: {
                    1: 614,
                    2: 615,
                    3: 616
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
                'landing.template.setSiteRef',
                [
                    'id' => 157,
                    'data' => [
                        1 => 614,
                        2 => 615,
                        3 => 616,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting site refs: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.template.setSiteRef',
        {
            id: 157,
            data: {
                1: 614,
                2: 615,
                3: 616
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
        'landing.template.setSiteRef',
        [
            'id' => 157,
            'data' => [
                1 => 614,
                2 => 615,
                3 => 616,
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
    "result": true,
    "time": {
        "start": 1774893504,
        "finish": 1774893504.131602,
        "duration": 0.13160204887390137,
        "processing": 0,
        "date_start": "2026-03-30T20:58:24+02:00",
        "date_finish": "2026-03-30T20:58:24+02:00",
        "operating_reset_at": 1774894104,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | The result of the call.

The method returns `true` if the request completed without error.

The method may return `true` if nothing has changed. For example, when the same bindings are already saved for the site, the user does not have permission to modify settings, or only values that are not saved are passed. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ENTITY_NOT_FOUND",
    "error_description": "Entity not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required parameter `id` is missing. ||
|| `ENTITY_NOT_FOUND` | The site is not found or is unavailable. ||
|| `ACCESS_DENIED` | The user does not have permission to modify site settings. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-template-get-landing-ref.md)
- [{#T}](./landing-template-get-site-ref.md)
- [{#T}](./landing-template-get-list.md)
- [{#T}](./landing-template-set-landing-ref.md)