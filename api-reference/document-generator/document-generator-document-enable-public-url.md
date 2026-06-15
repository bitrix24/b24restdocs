# Enable or disable public link for document documentgenerator.document.enablepublicurl

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the method: user with permission to modify documents

The method `documentgenerator.document.enablepublicurl` enables or disables the public link for a document.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Document identifier.

You can obtain the document identifier after [creating a document](./document-generator-document-add.md) or by using the [get document list method](./document-generator-document-list.md) ||
|| **status**
[`integer`](../data-types.md) | Status of the public link:
- `1` - enable
- `0` - disable

Default is `1` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":51,"status":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.document.enablepublicurl
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":51,"status":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.document.enablepublicurl
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
        method: 'documentgenerator.document.enablepublicurl',
        params: {
          id: 51,
          status: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Public URL:', result.publicUrl)
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
            method: 'documentgenerator.document.enablepublicurl',
            params: {
              id: 51,
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
          console.info('Public URL:', result.publicUrl)
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
      $response = $b24Service->core->call(
          'documentgenerator.document.enablepublicurl',
          [
              'id' => 51,
              'status' => 1,
          ]
      );

      $result = $response->getResponseData()->getResult();
      print_r($result);
  } catch (Throwable $e) {
      echo $e->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'documentgenerator.document.enablepublicurl',
      {
          id: 51,
          status: 1
      },
      function(result)
      {
          if (result.error())
          {
              console.error(result.error());
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

  $result = CRest::call(
      'documentgenerator.document.enablepublicurl',
      [
          'id' => 51,
          'status' => 1,
      ]
  );

  print_r($result);
  ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
            "publicUrl": "https://mysite.com/~nULUE"
    },
    "time": {
        "start": 1773913277,
        "finish": 1773913278.018128,
        "duration": 1.0181279182434082,
        "processing": 1,
        "date_start": "2026-03-19T12:41:17+02:00",
        "date_finish": "2026-03-19T12:41:18+02:00",
        "operating_reset_at": 1773913877,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **publicUrl**
[`string`](../data-types.md) | Public link to the document

If `status` is set to `0`, the field will return `null` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Document not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\DocumentGenerator\Document constructor must be is public | Required parameter `id` not provided ||
|| `400` | `0` | Document not found | Document with the specified `id` not found ||
|| `400` | `0` | You do not have permissions to view documents | Insufficient permissions to modify the document ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-document-add.md)
- [{#T}](./document-generator-document-update.md)
- [{#T}](./document-generator-document-get.md)
- [{#T}](./document-generator-document-list.md)
- [{#T}](./document-generator-document-delete.md)
- [{#T}](./document-generator-document-get-fields.md)