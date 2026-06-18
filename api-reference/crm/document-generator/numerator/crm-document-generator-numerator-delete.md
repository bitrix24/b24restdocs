# Delete crm.documentgenerator.numerator.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for document generator templates

The method `crm.documentgenerator.numerator.delete` removes a numerator.

You can only delete numerators created through [crm.documentgenerator.numerator.add](./crm-document-generator-numerator-add.md).

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the numerator ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

Example of deleting a numerator with `id = 47`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":47}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.numerator.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":47,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.numerator.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<null>({
        method: 'crm.documentgenerator.numerator.delete',
        params: {
          id: 47,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Numerator deleted:', result)
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
      async function deleteNumerator() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.documentgenerator.numerator.delete',
            params: {
              id: 47,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Numerator deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteNumerator)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.documentgenerator.numerator.delete',
                [
                    'id' => 47,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting numerator: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.numerator.delete',
        {
            id: 47,
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
        'crm.documentgenerator.numerator.delete',
        [
            'id' => 47,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": null,
    "time": {
        "start": 1773752251,
        "finish": 1773752251.165639,
        "duration": 0.16563892364501953,
        "processing": 0,
        "date_start": "2026-03-17T15:57:31+02:00",
        "date_finish": "2026-03-17T15:57:31+02:00",
        "operating_reset_at": 1773752851,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`null`](../../data-types.md) | Root element of the response. For the delete method, it returns `null` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Could not construct parameter {numerator}"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Could not construct parameter {numerator}` | The numerator with the specified `id` was not found or cannot be created from the provided data ||
|| `100` | `Bitrix\Main\Numerator\Numerator constructor must be is public` | The required parameter `id` for auto-binding the `numerator` object was not provided ||
|| `DOCGEN_ACCESS_ERROR` | `Access denied` | No access to the numerator: only REST numerators of the document generator type can be deleted ||
|| `Empty value` | `You do not have permissions to modify templates` | Insufficient rights to modify document generator templates ||
|| `Empty value` | `Module documentgenerator is not installed` | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-numerator-add.md)
- [{#T}](./crm-document-generator-numerator-update.md)
- [{#T}](./crm-document-generator-numerator-get.md)
- [{#T}](./crm-document-generator-numerator-list.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)