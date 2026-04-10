# Set a Special Page for the Site landing.syspage.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify settings" access permission for the site

The method `landing.syspage.set` assigns a special page for the site.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Site identifier.

The site identifier can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) or from the result of the method [landing.site.add](../../site/landing-site-add.md) ||
|| **type***
[`string`](../../../data-types.md) | Code for the special page.

Possible values:

- `mainpage` — main page
- `catalog` — catalog main page
- `personal` — personal section
- `cart` — shopping cart
- `order` — order checkout
- `payment` — payment page
- `compare` — comparison page
- `feedback` — feedback page

Leading and trailing spaces are trimmed. If a code outside the list is provided after this, the method will terminate without changes ||
|| **lid**
[`integer`](../../../data-types.md) | Identifier of the page to be set as special for the site.

This parameter is filled if the page needs to be designated as special. If the parameter is not provided, the method will remove the current binding for `type`.

The method will terminate without changes and without error in two cases: if there is no binding for the pair `id` and `type`, or if a page that does not exist or is not available for modification is provided.

Values `0`, `null`, and an empty string are not considered as the absence of a parameter. They are retained as `0`. For such values, access permissions for the page are not additionally checked ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 1390,
        "type": "personal",
        "lid": 8593
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.syspage.set.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 1390,
        "type": "personal",
        "lid": 8593,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.syspage.set.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.syspage.set',
    		{
    			id: 1390,
    			type: 'personal',
    			lid: 8593
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
                'landing.syspage.set',
                [
                    'id' => 1390,
                    'type' => 'personal',
                    'lid' => 8593,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting special page: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.syspage.set',
        {
            id: 1390,
            type: 'personal',
            lid: 8593
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
        'landing.syspage.set',
        [
            'id' => 1390,
            'type' => 'personal',
            'lid' => 8593,
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
        "start": 1774296164,
        "finish": 1774296164.30144,
        "duration": 0.3014400005340576,
        "processing": 0,
        "date_start": "2026-03-23T23:02:44+01:00",
        "date_finish": "2026-03-23T23:02:44+01:00",
        "operating_reset_at": 1774296764,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the call.

The method returns `true` if the call completed without error. This does not always mean that the binding was created, updated, or deleted. In some cases, the method may return `true` if nothing changed.

For example, if `type` is not supported, the site or page is unavailable, the page from `lid` is not found, or there is no binding to delete yet ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Insufficient call parameters, missing: id"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `id` or `type` not provided ||
|| `ACCESS_DENIED` | Insufficient general permissions to call `landing` methods ||
|| `SYSTEM_ERROR` | Unexpected internal error during method execution ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-syspage-get.md)
- [{#T}](./landing-syspage-get-special-page.md)
- [{#T}](./landing-syspage-delete-for-landing.md)
- [{#T}](./landing-syspage-delete-for-site.md)