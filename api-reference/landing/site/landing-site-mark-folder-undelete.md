# Restore Folder from Recycle Bin landing.site.markFolderUnDelete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "delete" access permission for the site to which the folder belongs

The method `landing.site.markFolderUnDelete` restores a folder from the recycle bin and removes the deletion mark from this folder and the pages within it.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **scope** 
[`string`](../../data-types.md) | Internal scope of the landing pages. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../types.md) ||
|| **id*** 
[`integer`](../../data-types.md) | Identifier of the folder.

The folder identifier can be obtained using the method [landing.site.getFolders](./landing-site-get-folders.md) ||
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'landing.site.markFolderUnDelete',
        params: {
          id: 737,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Folder restored from trash:', result)
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
      async function markFolderUnDelete() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.site.markFolderUnDelete',
            params: {
              id: 737,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Folder restored from trash:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', markFolderUnDelete)
    </script>
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