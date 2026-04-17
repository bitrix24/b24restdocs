# Add Folder to Site landing.site.addFolder

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.site.addFolder` creates a folder in the specified site and returns the identifier of the created folder.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../data-types.md) | Internal scope of the landing pages. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../types.md) ||
|| **siteId*** 
[`integer`](../../data-types.md) | Identifier of the site where the folder needs to be created.

The site identifier can be obtained using the method [landing.site.getList](./landing-site-get-list.md) or from the result of the method [landing.site.add](./landing-site-add.md) ||
|| **fields*** 
[`object`](../../data-types.md) | Set of fields for the folder being created [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TITLE*** 
[`string`](../../data-types.md) | Title of the folder, maximum length `255` characters ||
|| **CODE** 
[`string`](../../data-types.md) | Symbolic code of the folder for the URL, maximum length `255` characters. If not provided or empty, the code is generated from `TITLE` using transliteration.

If the code is empty after transliteration, a random string of `12` characters is created ||
|| **PARENT_ID** 
[`integer`](../../data-types.md) | Identifier of the parent folder. If the value is `0`, `null`, or empty, the folder is created at the root of the site ||
|| **ACTIVE** 
[`string`](../../data-types.md) | Flag indicating the folder's activity `Y/N`, default is `N` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "siteId": 1817,
        "fields": {
          "TITLE": "New Folder",
          "CODE": "new-folder",
          "ACTIVE": "Y",
          "PARENT_ID": 736
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.addFolder.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "siteId": 1817,
        "fields": {
          "TITLE": "New Folder",
          "CODE": "new-folder",
          "ACTIVE": "Y",
          "PARENT_ID": 736
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.addFolder.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.addFolder',
    		{
    			siteId: 1817,
    			fields: {
    				TITLE: 'New Folder',
    				CODE: 'new-folder',
    				ACTIVE: 'Y',
    				PARENT_ID: 736
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
                'landing.site.addFolder',
                [
                    'siteId' => 1817,
                    'fields' => [
                        'TITLE' => 'New Folder',
                        'CODE' => 'new-folder',
                        'ACTIVE' => 'Y',
                        'PARENT_ID' => 736,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding folder: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.addFolder',
        {
            siteId: 1817,
            fields: {
                TITLE: 'New Folder',
                CODE: 'new-folder',
                ACTIVE: 'Y',
                PARENT_ID: 736
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
        'landing.site.addFolder',
        [
            'siteId' => 1817,
            'fields' => [
                'TITLE' => 'New Folder',
                'CODE' => 'new-folder',
                'ACTIVE' => 'Y',
                'PARENT_ID' => 736,
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
    "result": 89,
    "time": {
        "start": 1773169396,
        "finish": 1773169396.875899,
        "duration": 0.875899076461792,
        "processing": 0,
        "date_start": "2026-03-10T22:03:16+01:00",
        "date_finish": "2026-03-10T22:03:16+01:00",
        "operating_reset_at": 1773169996,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created folder ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Site not found or access to it is denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Site not found or access to it is denied ||
|| `BX_EMPTY_REQUIRED` | Required field is not filled, such as `TITLE` or `CODE` ||
|| `FOLDER_IS_NOT_UNIQUE` | A folder with this name is already defined. This error occurs when there is a conflict with `CODE` within the site and parent folder ||
|| `SLASH_IS_NOT_ALLOWED` | The character `/` is passed in `fields.CODE` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-update-folder.md)
- [{#T}](./landing-site-get-folders.md)
- [{#T}](./landing-site-mark-folder-delete.md)