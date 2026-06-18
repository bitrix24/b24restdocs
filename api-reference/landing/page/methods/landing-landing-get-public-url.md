# Get Public URL of the Page landing.landing.getpublicurl

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Sites and Stores" section and the "view" permission for the site.

The method `landing.landing.getpublicurl` returns the complete public URL of the page.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of the landing pages. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **lid*** 
[`integer`](../../../data-types.md) | Identifier of the page.

The page identifier can be obtained using the method [landing.landing.getList](./landing-landing-get-list.md), as well as from the results of the methods [landing.landing.add](./landing-landing-add.md), [landing.landing.addByTemplate](./landing-landing-add-by-template.md), and [landing.landing.copy](./landing-landing-copy.md) ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.getpublicurl.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.getpublicurl.json"
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
        method: 'landing.landing.getpublicurl',
        params: {
          lid: 351,
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
      async function getLandingPublicUrl() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.landing.getpublicurl',
            params: {
              lid: 351,
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

      document.addEventListener('DOMContentLoaded', getLandingPublicUrl)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.landing.getpublicurl',
                [
                    'lid' => 351,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . $result;
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting landing public URL: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.getpublicurl',
        {
            lid: 351
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
        'landing.landing.getpublicurl',
        [
            'lid' => 351,
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
    "result": "https://b24-r8rseo.bitrix24site.com/spring-sale/",
    "time": {
        "start": 1773784381,
        "finish": 1773784381.945904,
        "duration": 0.945904016494751,
        "processing": 0,
        "date_start": "2026-03-18T00:53:01+01:00",
        "date_finish": "2026-03-18T00:53:01+01:00",
        "operating_reset_at": 1773784981,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name** 
`type` | **Description** ||
|| **result**
[`string`](../../../data-types.md) | Complete public URL of the page.

For the main page of the site and the index page of the folder, the method returns the address without the `CODE` of the page. If the page has the `RULE` field filled, the address is also returned without adding the `CODE`.

More about the `CODE` and `RULE` fields in the section [Page Object Fields](../fields.md) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

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
|| `MISSING_PARAMS` | Required parameter `lid` is missing ||
|| `LANDING_NOT_EXIST` | Page not found: the `lid` contains an identifier of a non-existent, deleted, or inaccessible page ||
|| `ACCESS_DENIED` | Insufficient permissions to call the method ||
|| `TYPE_ERROR` | Data type error in the method call parameters ||
|| `SYSTEM_ERROR` | Internal error while executing the method ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add.md)
- [{#T}](./landing-landing-copy.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-get-preview.md)
- [{#T}](./landing-landing-resolve-id-by-public-url.md)
- [{#T}](./landing-landing-update.md)