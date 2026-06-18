# Enable or Disable Public Link for Document crm.documentgenerator.document.enablepublicurl

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for document generator documents

The method `crm.documentgenerator.document.enablepublicurl` enables or disables the public link for a document.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../data-types.md) | Document identifier ||
|| **status**
[`integer`](../../data-types.md) | Public link mode:

- `1` — enable
- `0` — disable

Default: `1` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

The example enables the public link for document `61`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":61,"status":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.document.enablepublicurl
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":61,"status":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.document.enablepublicurl
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EnablePublicUrlResult = {
      publicUrl: string | null
    }

    try {
      const response = await $b24.actions.v2.call.make<EnablePublicUrlResult>({
        method: 'crm.documentgenerator.document.enablepublicurl',
        params: {
          id: 61,
          status: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.publicUrl)
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
      async function enablePublicUrl() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.documentgenerator.document.enablepublicurl',
            params: {
              id: 61,
              status: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.publicUrl)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', enablePublicUrl)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.documentgenerator.document.enablepublicurl',
                [
                    'id' => 61,
                    'status' => 1,
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
        echo 'Error enabling public URL: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.document.enablepublicurl',
        {
            id: 61,
            status: 1,
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
        'crm.documentgenerator.document.enablepublicurl',
        [
            'id' => 61,
            'status' => 1,
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
    "result": {
        "publicUrl": "https://bitrix.bitrix24.com/~71q5j"
    },
    "time": {
        "start": 1774011939,
        "finish": 1774011939.899233,
        "duration": 0.8992331027984619,
        "processing": 0,
        "date_start": "2026-03-20T16:05:39+01:00",
        "date_finish": "2026-03-20T16:05:39+01:00",
        "operating_reset_at": 1774012539,
        "operating": 0.3236720561981201
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the response. Contains the structure [`result`](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **publicUrl**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Public link to the document. Returns `null` when `status = 0` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Document not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | Bitrix\\DocumentGenerator\\Document constructor must be is public | Required parameter `id` not provided ||
|| `DOCGEN_ACCESS_ERROR` | Access denied | No access to the document ||
|| `0` | Document not found | Document with the specified `id` not found ||
|| `Empty value` | Document not found | Document does not belong to the `crm` module ||
|| `Empty value` | You do not have permissions to modify this document | Insufficient permissions to modify the document ||
|| `Empty value` | You do not have permissions to view documents | Insufficient permissions to view document generator documents ||
|| `Empty value` | Module documentgenerator is not installed | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-document-get.md)
- [{#T}](./crm-document-generator-document-update.md)
- [{#T}](./crm-document-generator-document-delete.md)
- [{#T}](./crm-document-generator-document-list.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)