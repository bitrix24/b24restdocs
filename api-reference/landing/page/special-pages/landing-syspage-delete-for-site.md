# Delete All Bindings of Special Pages for Site landing.syspage.deleteForSite

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "modify settings" access permission for the site

The method `landing.syspage.deleteForSite` removes all bindings of special pages for the site.

This method does not delete the site or the pages themselves. It only removes the records of the bindings of special pages. All found bindings for the specified site are deleted in a single call.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | The identifier of the site for which all bindings of special pages need to be deleted.

The site identifier can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) or from the result of the method [landing.site.add](../../site/landing-site-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 1390
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.syspage.deleteForSite.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 1390,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.syspage.deleteForSite.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.syspage.deleteForSite',
    		{
    			id: 1390
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
                'landing.syspage.deleteForSite',
                [
                    'id' => 1390,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting special page bindings for site: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.syspage.deleteForSite',
        {
            id: 1390
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
        'landing.syspage.deleteForSite',
        [
            'id' => 1390,
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
        "start": 1774368368,
        "finish": 1774368368.975637,
        "duration": 0.9756369590759277,
        "processing": 0,
        "date_start": "2026-03-24T19:06:08+01:00",
        "date_finish": "2026-03-24T19:06:08+01:00",
        "operating_reset_at": 1774368968,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the REST call completed without error.

This value does not confirm that the bindings were deleted. The method may return `true` without any changes.

For example, if the site is not found, moved to the trash, the user does not have the "modify settings" access permission for the site, or there are no special bindings for the site ||
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
|| `MISSING_PARAMS` | The required parameter `id` was not provided ||
|| `ACCESS_DENIED` | Insufficient general permissions to call `landing` methods ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-syspage-set.md)
- [{#T}](./landing-syspage-get.md)
- [{#T}](./landing-syspage-get-special-page.md)
- [{#T}](./landing-syspage-delete-for-landing.md)