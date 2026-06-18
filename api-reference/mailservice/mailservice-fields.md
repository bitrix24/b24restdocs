# Get Mail Service Fields mailservice.fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `mailservice.fields` returns localized names of the mail service fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/mailservice.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth": "**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/mailservice.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MailserviceFieldsResult = Record<string, string>

    try {
      const response = await $b24.actions.v2.call.make<MailserviceFieldsResult>({
        method: 'mailservice.fields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field labels:', result)
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
      async function getMailserviceFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'mailservice.fields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field labels:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMailserviceFields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call('mailservice.fields', []);

        $result = $response->getResponseData()->getResult();
        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'mailservice.fields',
        {},
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

    $result = CRest::call('mailservice.fields', []);
    print_r($result);
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.mailservice.fields().response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```
{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": "ID",
        "SITE_ID": "Site",
        "ACTIVE": "Activity",
        "NAME": "Name",
        "SERVER": "Server Address",
        "PORT": "Port",
        "ENCRYPTION": "Secure Connection",
        "LINK": "Web Interface Address",
        "ICON": "Logo",
        "SORT": "Sorting"
    },
    "time": {
        "start": 1710000500.123,
        "finish": 1710000500.200,
        "duration": 0.077,
        "processing": 0.020,
        "date_start": "2024-03-09T10:08:20+02:00",
        "date_finish": "2024-03-09T10:08:20+02:00",
        "operating_reset_at": 1710003600,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with field names. The structure of the object is described in detail [below](#fields-map) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#fields-map}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Name of the identifier field ||
|| **SITE_ID**
[`string`](../data-types.md) | Name of the site field ||
|| **ACTIVE**
[`string`](../data-types.md) | Name of the activity field ||
|| **NAME**
[`string`](../data-types.md) | Name of the service field ||
|| **SERVER**
[`string`](../data-types.md) | Name of the server address field ||
|| **PORT**
[`string`](../data-types.md) | Name of the port field ||
|| **ENCRYPTION**
[`string`](../data-types.md) | Name of the secure connection field ||
|| **LINK**
[`string`](../data-types.md) | Name of the web interface address field ||
|| **ICON**
[`string`](../data-types.md) | Name of the logo field ||
|| **SORT**
[`string`](../data-types.md) | Name of the sorting field ||
|#

## Error Handling

{% include notitle [Error Handling](../../_includes/error-info.md) %}

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mailservice-add.md)
- [{#T}](./mailservice-update.md)
- [{#T}](./mailservice-get.md)
- [{#T}](./mailservice-list.md)
- [{#T}](./mailservice-delete.md)