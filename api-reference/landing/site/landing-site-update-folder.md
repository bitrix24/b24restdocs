# Change Folder in landing.site.updateFolder

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.site.updateFolder` updates the parameters of a site folder.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../data-types.md) | Internal scope of the landing. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../types.md) ||
|| **siteId*** 
[`integer`](../../data-types.md) | Identifier of the site to which the folder belongs.

The site identifier can be obtained using the [landing.site.getList](./landing-site-get-list.md) method ||
|| **folderId*** 
[`integer`](../../data-types.md) | Identifier of the folder to be updated.

The folder identifier can be obtained using the [landing.site.getFolders](./landing-site-get-folders.md) method ||
|| **fields*** 
[`object`](../../data-types.md) | Set of fields to update the folder [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../../data-types.md) | Title of the folder ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the folder for the URL. The code cannot contain the character `/`. 

If the parameter is not provided, the current value of `CODE` for the folder is used ||
|| **PARENT_ID**
[`integer`](../../data-types.md) | Identifier of the parent folder. 

If the value is `0`, `null`, empty, or the parameter is not provided, the folder is moved to the root of the site ||
|| **INDEX_ID**
[`integer`](../../data-types.md) | Identifier of the index page of the folder ||
|| **ACTIVE**
[`string`](../../data-types.md) | Active flag for the folder `Y/N` ||
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
        "folderId": 736,
        "fields": {
          "TITLE": "Services Catalog",
          "CODE": "services-catalog",
          "PARENT_ID": 0,
          "INDEX_ID": 987,
          "ACTIVE": "Y"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.updateFolder.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "siteId": 1817,
        "folderId": 736,
        "fields": {
          "TITLE": "Services Catalog",
          "CODE": "services-catalog",
          "PARENT_ID": 0,
          "INDEX_ID": 987,
          "ACTIVE": "Y"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.updateFolder.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'landing.site.updateFolder',
        params: {
          siteId: 1817,
          folderId: 736,
          fields: {
            TITLE: 'Services Catalog',
            CODE: 'services-catalog',
            PARENT_ID: 0,
            INDEX_ID: 987,
            ACTIVE: 'Y',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Folder updated successfully:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function updateFolder() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.site.updateFolder',
            params: {
              siteId: 1817,
              folderId: 736,
              fields: {
                TITLE: 'Services Catalog',
                CODE: 'services-catalog',
                PARENT_ID: 0,
                INDEX_ID: 987,
                ACTIVE: 'Y',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Folder updated successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateFolder)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.site.updateFolder',
                [
                    'siteId' => 1817,
                    'folderId' => 736,
                    'fields' => [
                        'TITLE' => 'Services Catalog',
                        'CODE' => 'services-catalog',
                        'PARENT_ID' => 0,
                        'INDEX_ID' => 987,
                        'ACTIVE' => 'Y',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating folder: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.updateFolder',
        {
            siteId: 1817,
            folderId: 736,
            fields: {
                TITLE: 'Services Catalog',
                CODE: 'services-catalog',
                PARENT_ID: 0,
                INDEX_ID: 987,
                ACTIVE: 'Y'
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
        'landing.site.updateFolder',
        [
            'siteId' => 1817,
            'folderId' => 736,
            'fields' => [
                'TITLE' => 'Services Catalog',
                'CODE' => 'services-catalog',
                'PARENT_ID' => 0,
                'INDEX_ID' => 987,
                'ACTIVE' => 'Y',
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
    "result": true,
    "time": {
        "start": 1773291230,
        "finish": 1773291230.313858,
        "duration": 0.3138580322265625,
        "processing": 0,
        "date_start": "2026-03-12T07:53:50+01:00",
        "date_finish": "2026-03-12T07:53:50+01:00",
        "operating_reset_at": 1773291830,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the folder was successfully updated ||
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
|| `MISSING_PARAMS` | Required parameter `siteId`, `folderId`, or `fields` is missing ||
|| `ACCESS_DENIED` | Site not found or access to it is denied ||
|| `FOLDER_IS_NOT_UNIQUE` | A folder with this name already exists ||
|| `SLASH_IS_NOT_ALLOWED` | Slash is not allowed in the folder address ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add-folder.md)
- [{#T}](./landing-site-get-folders.md)
- [{#T}](./landing-site-mark-folder-delete.md)
- [{#T}](./landing-site-mark-folder-undelete.md)
- [{#T}](./landing-site-publication-folder.md)
- [{#T}](./landing-site-unpublic-folder.md)