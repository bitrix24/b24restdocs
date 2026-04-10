# Publish Site landing.site.publication

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with "publication" access permission for the site

The method `landing.site.publication` publishes the site and its pages. 
If a page is inactive (`ACTIVE = "N"`), it remains unpublished (`PUBLIC = "N"`). When the method is called, non-deleted folders of the site are also activated (`DELETED = "N"`).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the site.

The site identifier can be obtained using the method [landing.site.getList](./landing-site-get-list.md) or from the result of the method [landing.site.add](./landing-site-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 1688
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.publication.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 1688,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.publication.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.publication',
    		{
    			id: 1688
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
                'landing.site.publication',
                [
                    'id' => 1688,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error publishing site: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.publication',
        {
            id: 1688
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
        'landing.site.publication',
        [
            'id' => 1688,
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
    "result": 157,
    "time": {
        "start": 1773285117,
        "finish": 1773285117.990578,
        "duration": 0.9905779361724854,
        "processing": 0,
        "date_start": "2026-03-12T06:11:57+01:00",
        "date_finish": "2026-03-12T06:11:57+01:00",
        "operating_reset_at": 1773285717,
        "operating": 0.13959693908691406
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the published site ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Publishing the site is not allowed."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `id` is missing ||
|| `ACCESS_DENIED` | Publishing the site is not allowed ||
|| `PHONE_NOT_CONFIRMED` | Phone number confirmation is required for publishing ||
|| `EMAIL_NOT_CONFIRMED` | Email confirmation is required for publishing ||
|| `URLCHECKER_FAIL` | Malicious content detected on the page ||
|| `PUBLIC_PAGE_REACHED` | There is a limit on the number of published pages in the tariff plan ||
|| `LANDING_PAYMENT_FAILED` | The page was added from an application; a subscription to Bitrix24.Marketplace is required for publishing ||
|| `LANDING_PAYMENT_FAILED_BLOCK` | The block was added from an application; a subscription to Bitrix24.Marketplace is required for publishing ||
|| `PUBLIC_SITE_REACHED` | There is a limit on the number of created or published sites in the tariff plan ||
|| `PUBLIC_SITE_REACHED_FREE` | Publishing sites is temporarily available only on paid plans ||
|| `PUBLIC_HTML_DISALLOWED[...]` | There is a limit on adding custom HTML code in the tariff plan ||
|| `LICENSE_EXPIRED` | Your product license has expired ||
|| `TYPE_ERROR` | Data type error in method call parameters ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-get-public-url.md)
- [{#T}](./landing-site-publication-folder.md)
- [{#T}](./landing-site-unpublic.md)
- [{#T}](./landing-site-unpublic-folder.md)