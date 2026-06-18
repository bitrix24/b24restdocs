# Get Public URL of the Site landing.site.getPublicUrl

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Sites and Stores" section

The method `landing.site.getPublicUrl` returns the complete public URL of a site or multiple sites.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../data-types.md) | Internal scope of the landing pages. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../types.md) ||
|| **id*** 
[`integer`](../../data-types.md) \| [`array`](../../data-types.md) | Identifier of the site or an array of site identifiers. If a single identifier is provided, the `result` will return a URL string.

If an array of identifiers is provided, the `result` will return an object in the form `{"<ID>": "<URL>"}` only for the found sites.

The site identifier can be obtained using the method [landing.site.getList](./landing-site-get-list.md) or from the result of the method [landing.site.add](./landing-site-add.md) ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": [3, 135]
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.getPublicUrl.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": [3, 135],
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.getPublicUrl.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type GetPublicUrlResult = Record<string, string>

    try {
      const response = await $b24.actions.v2.call.make<GetPublicUrlResult>({
        method: 'landing.site.getPublicUrl',
        params: {
          id: [3, 135],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Public URLs by site ID:', result)
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
      async function getPublicUrl() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.site.getPublicUrl',
            params: {
              id: [3, 135],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Public URLs by site ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPublicUrl)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.site.getPublicUrl',
                [
                    'id' => [3, 135],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting public URL: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getPublicUrl',
        {
            id: [3, 135]
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
        'landing.site.getPublicUrl',
        [
            'id' => [3, 135],
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
    "result": {
        "3": "https://vilka.bitrix24.site",
        "135": "https://b24-odhzt3.bitrix24site.com"
    },
    "time": {
        "start": 1773277610,
        "finish": 1773277611.040785,
        "duration": 1.0407850742340088,
        "processing": 0,
        "date_start": "2026-03-12T04:06:50+01:00",
        "date_finish": "2026-03-12T04:06:51+01:00",
        "operating_reset_at": 1773278211,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../data-types.md) \| [`object`](../../data-types.md) \| [`array`](../../data-types.md) | The result depends on the type of the input parameter `id`.

If a single `id` is provided, the method returns a string with the site URL. If an array of `id` is provided, the method returns an object in the form `{"<ID>": "<URL>"}` [(detailed description)](#result-map).

If the site is not found, an empty string `""` is returned for a single `id`, and an empty array `[]` may be returned for an array of `id` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object for Array of IDs {#result-map}

#| 
|| **Name**
`type` | **Description** ||
|| **<Site ID>**
[`string`](../../data-types.md) | Full public URL of the site. The URL may be returned without a trailing `/` ||
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
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `id` is missing ||
|| `ACCESS_DENIED` | Insufficient permissions to call the method ||
|| `TYPE_ERROR` | Data type error in the method call parameters ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add.md)
- [{#T}](./landing-site-update.md)
- [{#T}](./landing-site-get-preview.md)
- [{#T}](./landing-site-get-list.md)
- [{#T}](./landing-site-get-folders.md)