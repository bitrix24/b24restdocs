# Get API Revisions im.revision.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.revision.get` returns the API revisions of the [IM module](../../settings/cloud-and-on-premise/on-premise/versions.md) for the current Bitrix24.

Use the revision values to check the compatibility of the client with the Bitrix24 server, especially on on-premise accounts where the module version may differ from the current one.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.revision.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.revision.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ImRevisionGetResult = {
      rest: number
      web: number
      mobile: number
      desktop: number
      im_revision_mobile: number
    }

    try {
      const response = await $b24.actions.v2.call.make<ImRevisionGetResult>({
        method: 'im.revision.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('IM revisions:', result.rest, result.web, result.mobile)
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
      async function getImRevisions() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.revision.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('IM revisions:', result.rest, result.web, result.mobile)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getImRevisions)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call('im.revision.get', []);

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.revision.get',
        {},
        function(result)
        {
            if (result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call('im.revision.get', []);

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Success: ' . print_r($result['result'], true);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "rest": 32,
        "web": 130,
        "mobile": 22,
        "desktop": 6,
        "im_revision_mobile": 22
    },
    "time": {
        "start": 1771599789,
        "finish": 1771599789.521072,
        "duration": 0.5210719108581543,
        "processing": 0,
        "date_start": "2026-02-20T18:03:09+01:00",
        "date_finish": "2026-02-20T18:03:09+01:00",
        "operating_reset_at": 1771600389,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root object with API revisions ||
|| **result.rest**
[`integer`](../data-types.md) | REST API IM revision ||
|| **result.web**
[`integer`](../data-types.md) | Web API IM revision ||
|| **result.mobile**
[`integer`](../data-types.md) | Mobile API IM revision ||
|| **result.desktop**
[`integer`](../data-types.md) | Desktop API IM revision ||
|| **result.im_revision_mobile**
[`integer`](../data-types.md) | Additional compatibility field, duplicates `result.mobile` ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

There are no specific business errors for this method.

{% include notitle [Error Handling](../../_includes/error-info.md) %}

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [im.counters.get](./im-counters-get.md)
- [im.recent.get](./im-recent-get.md)
- [im.dialog.get](./im-dialog-get.md)