# Get a List of Included Areas for the landing.template.getLandingRef Page

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" access permission for the site

The method `landing.template.getLandingRef` retrieves a list of included areas associated with the page.

The response includes only the bindings saved for the page. Bindings configured at the site level are not returned by this method. To obtain them, use the method [landing.template.getSiteRef](./landing-template-get-site-ref.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | The identifier of the page.

The page identifier can be obtained using the method [landing.landing.getList](../page/methods/landing-landing-get-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 557
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.template.getLandingRef.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 557,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.template.getLandingRef.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type LandingRefResult = Record<string, number> | true

    try {
      const response = await $b24.actions.v2.call.make<LandingRefResult>({
        method: 'landing.template.getLandingRef',
        params: {
          id: 557,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        if (result === true) {
          console.info('No landing refs configured')
        } else {
          console.info('Landing refs:', result, 'Area count:', Object.keys(result).length)
        }
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
      async function getLandingRef() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.template.getLandingRef',
            params: {
              id: 557,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          if (result === true) {
            console.info('No landing refs configured')
          } else {
            console.info('Landing refs:', result, 'Area count:', Object.keys(result).length)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getLandingRef)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.template.getLandingRef',
                [
                    'id' => 557,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting landing refs: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.template.getLandingRef',
        {
            id: 557
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
        'landing.template.getLandingRef',
        [
            'id' => 557,
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

If included areas are configured for the page, the method will return an object containing a list of these bindings. For each area, the identifier of the associated page is specified.

If no bindings for included areas are saved for the page, the method returns `true`.

```json
{
    "result": {
        "1": 614,
        "2": 615,
        "3": 616
    },
    "time": {
        "start": 1774765200,
        "finish": 1774765200.411258,
        "duration": 0.4112579822540283,
        "processing": 0,
        "date_start": "2026-03-29T10:00:00+02:00",
        "date_finish": "2026-03-29T10:00:00+02:00",
        "operating_reset_at": 1774765800,
        "operating": 0
    }
}
```

```json
{
    "result": true,
    "time": {
        "start": 1774874736,
        "finish": 1774874736.085915,
        "duration": 0.08591508865356445,
        "processing": 0,
        "date_start": "2026-03-30T15:45:36+02:00",
        "date_finish": "2026-03-30T15:45:36+02:00",
        "operating_reset_at": 1774875336,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) \| [`boolean`](../../data-types.md) | Bindings of included areas for the page in the format `{"<AREA_ID>": <PAGE_ID>}`.

If bindings are saved for the page, the method returns an object. If there are no bindings, the method returns `true` [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **<AREA_ID>**
[`integer`](../../data-types.md) | Identifier of the included area of the template. The value for this key contains the identifier of the page assigned to this area ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ENTITY_NOT_FOUND",
    "error_description": "Entity not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required parameter `id` was not provided ||
|| `ENTITY_NOT_FOUND` | The page was not found or is unavailable ||
|| `ACCESS_DENIED` | Insufficient rights to view the site ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-template-get-site-ref.md)
- [{#T}](./landing-template-get-list.md)
- [{#T}](./landing-template-set-landing-ref.md)
- [{#T}](./landing-template-set-site-ref.md)