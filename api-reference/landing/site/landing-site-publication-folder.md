# Publish a Website Folder landing.site.publicationFolder

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "publication" access permission for the website to which the folder belongs.

The method `landing.site.publicationFolder` publishes a website folder along with its chain of parent folders.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **folderId***
[`integer`](../../data-types.md) | The identifier of the folder.

The folder identifier can be obtained using the [landing.site.getFolders](./landing-site-get-folders.md) method. ||
|| **mark**
[`boolean`](../../data-types.md) | Publication flag. `true` (default) publishes the folder, `false` unpublishes the folder.

It is recommended to use [landing.site.unPublicFolder](./landing-site-unpublic-folder.md) for unpublishing. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "folderId": 737,
        "mark": true
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.publicationFolder.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "folderId": 737,
        "mark": true,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.publicationFolder.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.publicationFolder',
    		{
    			folderId: 737,
    			mark: true
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
                'landing.site.publicationFolder',
                [
                    'folderId' => 737,
                    'mark' => true,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error publishing folder: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.publicationFolder',
        {
            folderId: 737,
            mark: true
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
        'landing.site.publicationFolder',
        [
            'folderId' => 737,
            'mark' => true,
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
        "start": 1773285926,
        "finish": 1773285926.538981,
        "duration": 0.5389809608459473,
        "processing": 0,
        "date_start": "2026-03-12T06:25:26+01:00",
        "date_finish": "2026-03-12T06:25:26+01:00",
        "operating_reset_at": 1773286526,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the folder is successfully published. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Folder not found or access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `folderId` not provided. ||
|| `ACCESS_DENIED` | Folder not found, deleted, or access denied. ||
|| `TYPE_ERROR` | Data type error in method call parameters. ||
|| `SYSTEM_ERROR` | Internal error during method execution. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add-folder.md)
- [{#T}](./landing-site-update-folder.md)
- [{#T}](./landing-site-get-folders.md)
- [{#T}](./landing-site-mark-folder-delete.md)
- [{#T}](./landing-site-mark-folder-undelete.md)
- [{#T}](./landing-site-unpublic-folder.md)