# Delete Estimate crm.quote.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "delete" access permission for estimates

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.delete](../universal/crm-item-delete.md).

{% endnote %}

The method `crm.quote.delete` removes an estimate.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Identifier of the estimate.

The identifier can be obtained using the methods [crm.quote.list](./crm-quote-list.md) or [crm.quote.add](./crm-quote-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of deleting an estimate with `id = 43`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":43}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":43,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.delete
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
        method: 'crm.quote.delete',
        params: {
          id: 43,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Deleted:', result)
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
      async function deleteQuote() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.delete',
            params: {
              id: 43,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteQuote)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.delete',
                [
                    'id' => 43,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Deleted: ' . ($result ? 'true' : 'false');

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting estimate: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.delete',
        {
            id: 43,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.quote.delete',
        [
            'id' => 43,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773414644,
        "finish": 1773414644.363449,
        "duration": 0.3634490966796875,
        "processing": 0,
        "date_start": "2026-03-13T18:10:44+01:00",
        "date_finish": "2026-03-13T18:10:44+01:00",
        "operating_reset_at": 1773415244,
        "operating": 0.2564728260040283
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Root element of the response, returns `true` on success ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Element not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `ID is not defined or invalid.` | An incorrect `id` was provided ||
|| `-` | `Access denied.` | The user does not have permission to delete estimates ||
|| `ERROR_CORE` | `Element not found` | The estimate with the provided `id` was not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-add.md)
- [{#T}](./crm-quote-update.md)
- [{#T}](./crm-quote-get.md)
- [{#T}](./crm-quote-list.md)
- [{#T}](./crm-quote-fields.md)
- [{#T}](./crm-quote-product-rows-set.md)
- [{#T}](./crm-quote-product-rows-get.md)