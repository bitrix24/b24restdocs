# Move Page landing.landing.move

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" and "delete" permissions on the site

The method `landing.landing.move` moves a page to another site or folder.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of the landing pages. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid*** 
[`integer`](../../../data-types.md) | Identifier of the page to be moved.

The page identifier can be obtained using the method [landing.landing.getList](./landing-landing-get-list.md), as well as from the results of the methods [landing.landing.add](./landing-landing-add.md), [landing.landing.addByTemplate](./landing-landing-add-by-template.md), and [landing.landing.copy](./landing-landing-copy.md) ||
|| **toSiteId**
[`integer`](../../../data-types.md) | Identifier of the destination site.

If the parameter is not provided or equals `0`, the current site of the page is used.

The site identifier can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) ||
|| **toFolderId**
[`integer`](../../../data-types.md) | Identifier of the destination folder in the target site.

If the parameter is not provided or equals `0`, the page is moved to the root of the target site.

If you are moving the page to a folder, that folder must belong to the same site. You cannot move a page to another site and to a folder of that site in one go — the method will return an error. In such a case, first move the page to the desired site, and then in a separate call — to the desired folder.

The folder identifier can be obtained using the method [landing.site.getFolders](../../site/landing-site-get-folders.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 2227,
        "toSiteId": 157,
        "toFolderId": 95
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.move.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 2227,
        "toSiteId": 157,
        "toFolderId": 95,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.move.json"
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
        method: 'landing.landing.move',
        params: {
          lid: 2227,
          toSiteId: 157,
          toFolderId: 95,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Page moved successfully:', result)
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
      async function moveLandingPage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.landing.move',
            params: {
              lid: 2227,
              toSiteId: 157,
              toFolderId: 95,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Page moved successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', moveLandingPage)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.landing.move',
                [
                    'lid' => 2227,
                    'toSiteId' => 157,
                    'toFolderId' => 95,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error moving page: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.move',
        {
            lid: 2227,
            toSiteId: 157,
            toFolderId: 95
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
        'landing.landing.move',
        [
            'lid' => 2227,
            'toSiteId' => 157,
            'toFolderId' => 95,
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
        "start": 1773789756,
        "finish": 1773789756.4837,
        "duration": 0.4837000370025635,
        "processing": 0,
        "date_start": "2026-03-18T02:22:36+01:00",
        "date_finish": "2026-03-18T02:22:36+01:00",
        "operating_reset_at": 1773790356,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the move. Returns `true` on success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "FOLDER_NOT_FOUND",
    "error_description": "Folder not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid` is not provided ||
|| `LANDING_NOT_EXIST` | Page not found, deleted, or unavailable to the current user ||
|| `ACCESS_DENIED` | Insufficient permissions to modify the target site ||
|| `DELETE_ACCESS_DENIED` | Insufficient permissions to delete the source page ||
|| `FOLDER_NOT_FOUND` | Target folder not found or inaccessible for verification. This error also occurs if you attempt to change the page's site and provide a folder from another site in one call ||
|| `TYPE_ERROR` | Data type error in method call parameters ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add.md)
- [{#T}](./landing-landing-add-by-template.md)
- [{#T}](./landing-landing-copy.md)
- [{#T}](./landing-landing-delete.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-mark-delete.md)
- [{#T}](./landing-landing-update.md)