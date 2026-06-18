# Copy Page landing.landing.copy

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.landing.copy` copies a page and returns the identifier of the new page. This method can also copy a page marked as deleted and located in the trash.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of landings. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid***
[`integer`](../../../data-types.md) | Identifier of the source page.

The page identifier can be obtained using the method [landing.landing.getList](./landing-landing-get-list.md) ||
|| **toSiteId**
[`integer`](../../../data-types.md) | Identifier of the target site. If this parameter is not provided or set to `0`, the copy will be created in the same site where the source page is located.

The site identifier can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) ||
|| **toFolderId**
[`integer`](../../../data-types.md) | Identifier of the target folder. If this parameter is provided, the copy is created in the specified folder. The folder must belong to the target site `toSiteId`.

If this parameter is not provided or set to `0`, the copy is created in the root of the target site. If the folder is not found or belongs to another site, the method does not return an error and creates the copy in the root of the target site.

The folder identifier can be obtained using the method [landing.site.getFolders](../../site/landing-site-get-folders.md) ||
|| **skipSystem**
[`boolean`](../../../data-types.md) | Flag for copying the system attribute of the page. Default is `false`.

If set to `true`, the new page is created as non-system (`SYS = N`)
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 1688,
        "toSiteId": 305,
        "toFolderId": 95
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.copy.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 1688,
        "toSiteId": 305,
        "toFolderId": 95,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.copy.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'landing.landing.copy',
        params: {
          lid: 1688,
          toSiteId: 305,
          toFolderId: 95,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('New page ID:', result)
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
      async function copyLandingPage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.landing.copy',
            params: {
              lid: 1688,
              toSiteId: 305,
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
          console.info('New page ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', copyLandingPage)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.landing.copy',
                [
                    'lid' => 1688,
                    'toSiteId' => 305,
                    'toFolderId' => 95,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error copying landing page: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.copy',
        {
            lid: 1688,
            toSiteId: 305,
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
        'landing.landing.copy',
        [
            'lid' => 1688,
            'toSiteId' => 305,
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
    "result": 2237,
    "time": {
        "start": 1773706491,
        "finish": 1773706492.91233,
        "duration": 1.912329912185669,
        "processing": 1,
        "date_start": "2026-03-17T03:14:51+01:00",
        "date_finish": "2026-03-17T03:14:52+01:00",
        "operating_reset_at": 1773707091,
        "operating": 0.9924919605255127
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created copy of the page ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400 Bad Request**

```json
{
    "error": "LANDING_NOT_EXIST",
    "error_description": "Landing not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `LANDING_NOT_EXIST` | Landing not found. If the source page does not exist or is not accessible to the current user. Being in the trash does not trigger this error by itself ||
|| `SITE_NOT_FOUND` | Site not found. If `toSiteId` points to a non-existent site ||
|| `ACCESS_DENIED` | Access to create the page is denied. If the user does not have "edit" access permission for the site ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add.md)
- [{#T}](./landing-landing-add-by-template.md)
- [{#T}](./landing-landing-delete.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-move.md)
- [{#T}](./landing-landing-publication.md)
- [{#T}](./landing-landing-update.md)