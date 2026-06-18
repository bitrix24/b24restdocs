# Get URL Preview of the Site landing.site.getPreview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" access permission for sites

The method `landing.site.getPreview` returns the URL preview of the site's index page.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **scope** 
[`string`](../../data-types.md) | Internal scope of the landing pages. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../types.md) ||
|| **id*** 
[`integer`](../../data-types.md) | Site identifier.

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
        "id": 1817
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.getPreview.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 1817,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.getPreview.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<string>({
        method: 'landing.site.getPreview',
        params: {
          id: 1817,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result)
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
      async function getSitePreview() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.site.getPreview',
            params: {
              id: 1817,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSitePreview)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.site.getPreview',
                [
                    'id' => 1817,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . $result;
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting site preview: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getPreview',
        {
            id: 1817
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
        'landing.site.getPreview',
        [
            'id' => 1817,
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo $result['result'];
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": "/preview.jpg",
    "time": {
        "start": 1773275667,
        "finish": 1773275667.66318,
        "duration": 0.6631801128387451,
        "processing": 0,
        "date_start": "2026-03-12T03:34:27+01:00",
        "date_finish": "2026-03-12T03:34:27+01:00",
        "operating_reset_at": 1773276267,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name** 
`type` | **Description** ||
|| **result** 
[`string`](../../data-types.md) | URL or relative path to the preview image of the site's index page.

The method returns a string with the path or URL of the preview. Common variants: `"/preview.jpg"` and `"/bitrix/images/landing/nopreview.jpg"` ||
|| **time** 
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough parameters for the call, missing: id"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `MISSING_PARAMS` | error_description | Not enough parameters for the call, missing: `id` ||
|| `400` | `ACCESS_DENIED` | error_description | Not enough rights to call the method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add.md)
- [{#T}](./landing-site-update.md)
- [{#T}](./landing-site-get-list.md)
- [{#T}](./landing-site-get-public-url.md)
- [{#T}](./landing-site-get-folders.md)