# Restore Folder from Recycle Bin landing.site.markFolderUnDelete

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "delete" access permission for the site to which the folder belongs

The method `landing.site.markFolderUnDelete` restores a folder from the recycle bin and removes the deletion flag from this folder and the pages within it.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the folder.

The folder identifier can be obtained using the [landing.site.getFolders](./landing-site-get-folders.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 737
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.markFolderUnDelete.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 737,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.markFolderUnDelete.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.markFolderUnDelete',
    		{
    			id: 737
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
                'landing.site.markFolderUnDelete',
                [
                    'id' => 737,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error restoring folder from recycle bin: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.markFolderUnDelete',
        {
            id: 737
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
        'landing.site.markFolderUnDelete',
        [
            'id' => 737,
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
        "start": 1773283571,
        "finish": 1773283571.157019,
        "duration": 0.15701889991760254,
        "processing": 0,
        "date_start": "2026-03-12T05:46:11+01:00",
        "date_finish": "2026-03-12T05:46:11+01:00",
        "operating_reset_at": 1773284171,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the folder was successfully restored from the recycle bin ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Folder not found or access to it is denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `id` is missing ||
|| `ACCESS_DENIED` | Folder not found or access to it is denied ||
|| `TYPE_ERROR` | Data type error in the method call parameters ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add-folder.md)
- [{#T}](./landing-site-update-folder.md)
- [{#T}](./landing-site-get-folders.md)
- [{#T}](./landing-site-mark-folder-delete.md)
- [{#T}](./landing-site-publication-folder.md)
- [{#T}](./landing-site-unpublic-folder.md)