# Create Mail Service mailservice.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `mailservice.add` creates a mail service for the current Bitrix24.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../data-types.md) | Name of the mail service ||
|| **ACTIVE**
[`string`](../data-types.md) | Service activity status. Allowed values:
- `Y` — yes
- `N` — no

Default value: `Y` ||
|| **SERVER**
[`string`](../data-types.md) | IMAP server address. Stored in lowercase in the database ||
|| **PORT**
[`integer`](../data-types.md) | IMAP server port ||
|| **ENCRYPTION**
[`string`](../data-types.md) | Secure connection status. Allowed values:
- `Y` — yes
- `N` — no ||
|| **LINK**
[`string`](../data-types.md) | Web interface address of the mail service ||
|| **ICON**
[`file`](../data-types.md)
[`integer`](../data-types.md)
[`string`](../data-types.md) | Service logo. You can provide a file or an existing file ID ||
|| **SORT**
[`integer`](../data-types.md) | Sort index. Default value: `100` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "NAME": "My Mail Service",
        "ACTIVE": "Y",
        "SERVER": "imap.my-mail.com",
        "PORT": 993,
        "ENCRYPTION": "Y",
        "LINK": "https://mail.my-mail.com/",
        "SORT": 500
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/mailservice.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "NAME": "My Mail Service",
        "ACTIVE": "Y",
        "SERVER": "imap.my-mail.com",
        "PORT": 993,
        "ENCRYPTION": "Y",
        "LINK": "https://mail.my-mail.com/",
        "SORT": 500,
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/mailservice.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'mailservice.add',
        params: {
          NAME: 'My mail service',
          ACTIVE: 'Y',
          SERVER: 'imap.my-mail.ru',
          PORT: 993,
          ENCRYPTION: 'Y',
          LINK: 'https://mail.my-mail.ru/',
          SORT: 500,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created mail service ID:', result)
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
      async function addMailService() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'mailservice.add',
            params: {
              NAME: 'My mail service',
              ACTIVE: 'Y',
              SERVER: 'imap.my-mail.ru',
              PORT: 993,
              ENCRYPTION: 'Y',
              LINK: 'https://mail.my-mail.ru/',
              SORT: 500,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created mail service ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addMailService)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mailservice.add',
                [
                    'NAME' => 'My Mail Service',
                    'ACTIVE' => 'Y',
                    'SERVER' => 'imap.my-mail.com',
                    'PORT' => 993,
                    'ENCRYPTION' => 'Y',
                    'LINK' => 'https://mail.my-mail.com/',
                    'SORT' => 500,
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
        'mailservice.add',
        {
            NAME: 'My Mail Service',
            ACTIVE: 'Y',
            SERVER: 'imap.my-mail.com',
            PORT: 993,
            ENCRYPTION: 'Y',
            LINK: 'https://mail.my-mail.com/',
            SORT: 500
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
        'mailservice.add',
        [
            'NAME' => 'My Mail Service',
            'ACTIVE' => 'Y',
            'SERVER' => 'imap.my-mail.com',
            'PORT' => 993,
            'ENCRYPTION' => 'Y',
            'LINK' => 'https://mail.my-mail.com/',
            'SORT' => 500,
        ]
    );

    print_r($result);
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.mailservice.add(
            name="SDK Mail Service",
            encryption=True,
            active=True,
            server="imap.example.com",
            port=993,
            link="https://mail.example.com",
            sort=200,
        ).response
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
    "result": 31,
    "time": {
        "start": 1774005930,
        "finish": 1774005930.256403,
        "duration": 0.25640296936035156,
        "processing": 0,
        "date_start": "2026-03-20T14:25:30+02:00",
        "date_finish": "2026-03-20T14:25:30+02:00",
        "operating_reset_at": 1774006530,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the created mail service ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Access denied"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CORE` | Access denied | Insufficient permissions to add the mail service ||
|| `400` | `ERROR_CORE` | Required field "Name" not filled | Required parameter `NAME` not provided ||
|| `400` | `ERROR_CORE` | Invalid value for "*field_name*" | Invalid value provided for the specified field ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mailservice-update.md)
- [{#T}](./mailservice-get.md)
- [{#T}](./mailservice-list.md)
- [{#T}](./mailservice-delete.md)
- [{#T}](./mailservice-fields.md)