# Set Included Areas for landing.template.setLandingRef Page

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify settings" access permission for the page

The method `landing.template.setLandingRef` sets the bindings of included areas for the page. It only works with page bindings and does not alter site bindings.

Included areas of the template are separate pages used as parts of the layout, such as the header, footer, or sidebar.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the page.

The page identifier can be obtained using the [landing.landing.getList](../page/methods/landing-landing-get-list.md) method. ||
|| **data**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Set of bindings for the included areas of the page. [(detailed description)](#data)

Provide the complete final set of bindings for the page. The method will update existing bindings, add new ones, and remove those not present in `data`.

If the parameter is not provided, an empty object `{}` or an empty array `[]` is passed, the method will remove all current bindings of included areas for the page. ||
|#

### Parameter data {#data}

#|
|| **Name**
`type` | **Description** ||
|| **<AREA_ID>**
[`integer`](../../data-types.md) | Identifier of the page to be assigned to the corresponding included area.

The key is the identifier of the template area, and the value is the identifier of the page.

Area identifiers depend on the page template. They can be determined from the template, for example, through the [landing.template.getlist](./landing-template-get-list.md) method and the `CONTENT` field: in the markup, areas are denoted as `#AREA_1#`, `#AREA_2#`, and so on.

In `data`, only the numeric part of such an identifier should be passed: `1`, `2`.

If a key for a saved area is not provided in `data` or if a value of `0`, an empty string, `null`, or a negative number is passed for it, the binding will be removed.

Ensure to provide correct identifiers for the area and page. If such an area does not exist in the template or a page with such an identifier does not exist, the method will not report an error. ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 557,
        "data": {
          "1": 614,
          "2": 615,
          "3": 616
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.template.setLandingRef.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 557,
        "data": {
          "1": 614,
          "2": 615,
          "3": 616
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.template.setLandingRef.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.template.setLandingRef',
            {
                id: 557,
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
                'landing.template.setLandingRef',
                [
                    'id' => 557,
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
        echo 'Error setting landing refs: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.template.setLandingRef',
        {
            id: 557,
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
        'landing.template.setLandingRef',
        [
            'id' => 557,
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
        "start": 1774891241,
        "finish": 1774891242.107728,
        "duration": 1.1077280044555664,
        "processing": 0,
        "date_start": "2026-03-30T20:20:41+01:00",
        "date_finish": "2026-03-30T20:20:42+01:00",
        "operating_reset_at": 1774891842,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the call.

The method returns `true` if the request is processed without error. If the same bindings are already saved for the page, the method will return `true`.

If the user does not have "modify settings" access permission for the page, the changes will not be applied. In this case, the method will return `true`. ||
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
|| `MISSING_PARAMS` | Required parameter `id` is missing. ||
|| `ENTITY_NOT_FOUND` | Page not found or unavailable. ||
|| `ACCESS_DENIED` | Insufficient rights to modify page settings. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-template-get-landing-ref.md)
- [{#T}](./landing-template-get-site-ref.md)
- [{#T}](./landing-template-get-list.md)
- [{#T}](./landing-template-set-site-ref.md)