# Mark the site as deleted landing.site.markDelete

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with "delete" access permission for sites

The method `landing.site.markDelete` marks the site as deleted and moves it to the trash.

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
        "id": 143
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.markDelete.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 143,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.markDelete.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.markDelete',
    		{
    			id: 143
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
                'landing.site.markDelete',
                [
                    'id' => 143,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error marking site as deleted: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.markDelete',
        {
            id: 143
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
        'landing.site.markDelete',
        [
            'id' => 143,
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
    "result": 143,
    "time": {
        "start": 1773282028,
        "finish": 1773282028.948274,
        "duration": 0.9482738971710205,
        "processing": 0,
        "date_start": "2026-03-12T05:20:28+01:00",
        "date_finish": "2026-03-12T05:20:28+01:00",
        "operating_reset_at": 1773282628,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the site marked as deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access to delete the site is denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `id` is missing ||
|| `ACCESS_DENIED` | Insufficient rights to modify the `DELETED` field of the site ||
|| `ACCESS_DENIED_DELETED` | Deleting the site is not available when a domain provider is connected in Bitrix24 ||
|| `TYPE_ERROR` | Data type error in the method call parameters ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add.md)
- [{#T}](./landing-site-update.md)
- [{#T}](./landing-site-get-list.md)
- [{#T}](./landing-site-delete.md)
- [{#T}](./landing-site-mark-undelete.md)