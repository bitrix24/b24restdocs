# Get URL Preview of the landing.site.getPreview

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with "view" access permission for sites

The method `landing.site.getPreview` returns the URL of the preview for the site's index page.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Site identifier.

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
        "id": 1817
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.getPreview.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 1817,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.getPreview.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.getPreview',
    		{
    			id: 1817
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
                'landing.site.getPreview',
                [
                    'id' => 1817,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . $result;
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting site preview: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getPreview',
        {
            id: 1817
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
        'landing.site.getPreview',
        [
            'id' => 1817,
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo $result['result'];
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": "/preview.jpg",
    "time": {
        "start": 1773275667,
        "finish": 1773275667.66318,
        "duration": 0.6631801128387451,
        "processing": 0,
        "date_start": "2026-03-12T03:34:27+01:00",
        "date_finish": "2026-03-12T03:34:27+01:00",
        "operating_reset_at": 1773276267,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../data-types.md) | URL or relative path to the preview image of the site's index page. 

The method returns a string with the path or URL of the preview. Common variants include: `"/preview.jpg"` and `"/bitrix/images/landing/nopreview.jpg"` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Insufficient parameters for the call, missing: id"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `MISSING_PARAMS` | error_description | Insufficient parameters for the call, missing: `id` ||
|| `400` | `ACCESS_DENIED` | error_description | Insufficient rights to call the method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add.md)
- [{#T}](./landing-site-update.md)
- [{#T}](./landing-site-get-list.md)
- [{#T}](./landing-site-get-public-url.md)
- [{#T}](./landing-site-get-folders.md)