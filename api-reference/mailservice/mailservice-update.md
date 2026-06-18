# Update Mail Service mailservice.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `mailservice.update` modifies the parameters of an existing mail service.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../data-types.md) | Identifier of the mail service. 

You can obtain the identifier using the [mailservice.list](./mailservice-list.md) method. ||
|| **ACTIVE**
[`string`](../data-types.md) | Service activity status. Acceptable values:
- `Y` — yes
- `N` — no ||
|| **NAME**
[`string`](../data-types.md) | Name of the service ||
|| **SERVER**
[`string`](../data-types.md) | IMAP server address. Stored in lowercase in the database. ||
|| **PORT**
[`integer`](../data-types.md) | IMAP server port ||
|| **ENCRYPTION**
[`string`](../data-types.md) | Secure connection status. Acceptable values:
- `Y` — yes
- `N` — no ||
|| **LINK**
[`string`](../data-types.md) | Web interface address of the service ||
|| **ICON**
[`file`](../data-types.md)
[`integer`](../data-types.md)
[`string`](../data-types.md) | Service logo. You can provide a file or an existing file identifier. ||
|| **SORT**
[`integer`](../data-types.md) | Sort index ||
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
        "ID": 31,
        "NAME": "Updated Mail Service",
        "ACTIVE": "N",
        "SERVER": "imap.my2-mail.com",
        "PORT": 993,
        "ENCRYPTION": "Y",
        "LINK": "https://mail.my2-mail.com/",
        "SORT": 600
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/mailservice.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "ID": 31,
        "NAME": "Updated Mail Service",
        "ACTIVE": "N",
        "SERVER": "imap.my2-mail.com",
        "PORT": 993,
        "ENCRYPTION": "Y",
        "LINK": "https://mail.my2-mail.com/",
        "SORT": 600,
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/mailservice.update
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
        method: 'mailservice.update',
        params: {
          ID: 31,
          NAME: 'Updated mail service',
          ACTIVE: 'N',
          SERVER: 'imap.my2-mail.ru',
          PORT: 993,
          ENCRYPTION: 'Y',
          LINK: 'https://mail.my2-mail.ru/',
          SORT: 600,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Mail service updated:', result)
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
      async function updateMailService() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'mailservice.update',
            params: {
              ID: 31,
              NAME: 'Updated mail service',
              ACTIVE: 'N',
              SERVER: 'imap.my2-mail.ru',
              PORT: 993,
              ENCRYPTION: 'Y',
              LINK: 'https://mail.my2-mail.ru/',
              SORT: 600,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Mail service updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateMailService)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mailservice.update',
                [
                    'ID' => 31,
                    'NAME' => 'Updated Mail Service',
                    'ACTIVE' => 'N',
                    'SERVER' => 'imap.my2-mail.com',
                    'PORT' => 993,
                    'ENCRYPTION' => 'Y',
                    'LINK' => 'https://mail.my2-mail.com/',
                    'SORT' => 600,
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
        'mailservice.update',
        {
            ID: 31,
            NAME: 'Updated Mail Service',
            ACTIVE: 'N',
            SERVER: 'imap.my2-mail.com',
            PORT: 993,
            ENCRYPTION: 'Y',
            LINK: 'https://mail.my2-mail.com/',
            SORT: 600
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
        'mailservice.update',
        [
            'ID' => 31,
            'NAME' => 'Updated Mail Service',
            'ACTIVE' => 'N',
            'SERVER' => 'imap.my2-mail.com',
            'PORT' => 993,
            'ENCRYPTION' => 'Y',
            'LINK' => 'https://mail.my2-mail.com/',
            'SORT' => 600,
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
        bitrix_response = client.mailservice.update(
            bitrix_id=31,
            active=True,
            name="SDK Mail Service Updated",
            server="imap-updated.example.com",
            port=993,
            encryption=True,
            link="https://mail-updated.example.com",
            sort=300,
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

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1774008238,
        "finish": 1774008238.539154,
        "duration": 0.539154052734375,
        "processing": 0,
        "date_start": "2026-03-20T15:03:58+02:00",
        "date_finish": "2026-03-20T15:03:58+02:00",
        "operating_reset_at": 1774008838,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if the service was successfully updated ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Mail service not found"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CORE` | Access denied | Insufficient rights to add a mail service ||
|| `400` | `ERROR_CORE` | Mail service ID not specified | Required parameter `ID` not provided ||
|| `400` | `ERROR_CORE` | Mail service not found | Mail service with the specified `ID` not found ||
|| `400` | `ERROR_CORE` | Invalid value for "*field_name*" | An invalid value for the specified field was provided ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mailservice-add.md)
- [{#T}](./mailservice-get.md)
- [{#T}](./mailservice-list.md)
- [{#T}](./mailservice-delete.md)
- [{#T}](./mailservice-fields.md)