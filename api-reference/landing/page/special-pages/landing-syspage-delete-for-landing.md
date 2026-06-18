# Delete All Page Bindings as Special landing.syspage.deleteForLanding

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify site settings" access permission

The method `landing.syspage.deleteForLanding` removes all bindings where the page with the identifier `id` is designated as special for the site.

This method does not delete sites or pages. It only removes bindings of special pages. If the same page is designated as special for multiple sites, the method will delete all such bindings at once.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | The identifier of the page for which all special bindings need to be removed.

The page identifier can be obtained using the [landing.landing.getList](../methods/landing-landing-get-list.md) method, as well as from the results of the [landing.landing.add](../methods/landing-landing-add.md), [landing.landing.addByTemplate](../methods/landing-landing-add-by-template.md), and [landing.landing.copy](../methods/landing-landing-copy.md) methods ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 8593
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.syspage.deleteForLanding.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 8593,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.syspage.deleteForLanding.json"
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
        method: 'landing.syspage.deleteForLanding',
        params: {
          id: 8593,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Bindings deleted:', result)
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
      async function deleteForLanding() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.syspage.deleteForLanding',
            params: {
              id: 8593,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Bindings deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteForLanding)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.syspage.deleteForLanding',
                [
                    'id' => 8593,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting special page bindings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.syspage.deleteForLanding',
        {
            id: 8593
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
        'landing.syspage.deleteForLanding',
        [
            'id' => 8593,
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
        "start": 1774365845,
        "finish": 1774365845.959878,
        "duration": 0.9598779678344727,
        "processing": 0,
        "date_start": "2026-03-24T18:24:05+01:00",
        "date_finish": "2026-03-24T18:24:05+01:00",
        "operating_reset_at": 1774366445,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the REST call completed without error.

This value does not confirm that the bindings were deleted. The method may return `true` without changes, for example, if the page is not found, the user does not have access permission for the page's site, or there are no special bindings for the page ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Insufficient call parameters, missing: id"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required parameter `id` was not provided ||
|| `ACCESS_DENIED` | Insufficient general permissions to call `landing` methods ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-syspage-set.md)
- [{#T}](./landing-syspage-get.md)
- [{#T}](./landing-syspage-get-special-page.md)
- [{#T}](./landing-syspage-delete-for-site.md)