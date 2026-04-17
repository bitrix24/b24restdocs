# Get Page ID by Public URL landing.landing.resolveIdByPublicUrl

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Sites and Stores" section

The method `landing.landing.resolveIdByPublicUrl` returns the page ID based on its public URL within the specified site.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of the landing pages. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **landingUrl*** 
[`string`](../../../data-types.md) | Relative public URL of the page within the site `siteId`, for example `/catalog/sale/`.

Provide the path from the root of the site without the domain ||
|| **siteId*** 
[`integer`](../../../data-types.md) | The ID of the site within which to find the page.

The site ID can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "landingUrl": "/catalog/sale/",
        "siteId": 1817
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.resolveIdByPublicUrl.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "landingUrl": "/catalog/sale/",
        "siteId": 1817,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.resolveIdByPublicUrl.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.resolveIdByPublicUrl',
    		{
    			landingUrl: '/catalog/sale/',
    			siteId: 1817
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
                'landing.landing.resolveIdByPublicUrl',
                [
                    'landingUrl' => '/catalog/sale/',
                    'siteId' => 1817,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error resolving landing ID: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.resolveIdByPublicUrl',
        {
            landingUrl: '/catalog/sale/',
            siteId: 1817
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
        'landing.landing.resolveIdByPublicUrl',
        [
            'landingUrl' => '/catalog/sale/',
            'siteId' => 1817,
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
    "result": 2231,
    "time": {
        "start": 1773830531,
        "finish": 1773830531.858353,
        "duration": 0.8583528995513916,
        "processing": 0,
        "date_start": "2026-03-18T13:42:11+03:00",
        "date_finish": "2026-03-18T13:42:11+03:00",
        "operating_reset_at": 1773831131,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) \| `null` | The ID of the found page.

If a page with such a public URL is not found on the site `siteId`, the method returns `null` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough call parameters, missing: landingUrl, siteId"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameters `landingUrl`, `siteId` or one of them are not provided ||
|| `ACCESS_DENIED` | Insufficient permissions to call the method ||
|| `TYPE_ERROR` | Data type error in the method call parameters ||
|| `SYSTEM_ERROR` | Internal error while executing the method ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-get-preview.md)
- [{#T}](./landing-landing-get-public-url.md)
- [{#T}](./landing-landing-move.md)
- [{#T}](./landing-landing-update.md)