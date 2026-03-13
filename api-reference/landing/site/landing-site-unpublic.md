# Unpublishing a Site landing.site.unpublic

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "publication" access permission for the site

The method `landing.site.unpublic` unpublishes the site and its pages, deactivating the site. For all non-deleted folders of the site, the method also disables activity.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | The identifier of the site.

The site identifier can be obtained using the [landing.site.getList](./landing-site-get-list.md) method or from the result of the [landing.site.add](./landing-site-add.md) method ||
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
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.unpublic.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 1688,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.unpublic.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.unpublic',
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
                'landing.site.unpublic',
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
        echo 'Error unpublishing site: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.unpublic',
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
        'landing.site.unpublic',
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
        "start": 1773287437,
        "finish": 1773287437.267949,
        "duration": 0.26794910430908203,
        "processing": 0,
        "date_start": "2026-03-12T06:50:37+01:00",
        "date_finish": "2026-03-12T06:50:37+01:00",
        "operating_reset_at": 1773288037,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | The identifier of the unpublished site ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Publishing access for the site is denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required parameter `id` is missing ||
|| `ACCESS_DENIED` | Insufficient permissions to modify the `ACTIVE` field of the site ||
|| `TYPE_ERROR` | Data type error in the method call parameters ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-publication.md)
- [{#T}](./landing-site-get-public-url.md)
- [{#T}](./landing-site-publication-folder.md)
- [{#T}](./landing-site-unpublic-folder.md)