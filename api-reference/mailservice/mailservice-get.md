# Get the mailservice.get method

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `mailservice.get` method returns the parameters of the mail service by its identifier.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../data-types.md) | Identifier of the mail service.

You can obtain the identifier using the [mailservice.list](./mailservice-list.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID": 31}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/mailservice.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID": 31, "auth": "**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/mailservice.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MailServiceGetResult = {
      ID: string
      SITE_ID: string
      ACTIVE: string
      SORT: string
      NAME: string
      SERVER: string
      PORT: string
      ENCRYPTION: string
      LINK: string
      ICON: string | null
      SMTP_SERVER: string | null
      SMTP_PORT: string | null
      SMTP_LOGIN_AS_IMAP: string
      SMTP_PASSWORD_AS_IMAP: string
      SMTP_ENCRYPTION: string | null
      UPLOAD_OUTGOING: string | null
    }

    try {
      const response = await $b24.actions.v2.call.make<MailServiceGetResult>({
        method: 'mailservice.get',
        params: {
          ID: 31,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.ID, result.NAME, result.SERVER)
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
      async function getMailService() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'mailservice.get',
            params: {
              ID: 31,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.ID, result.NAME, result.SERVER)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMailService)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mailservice.get',
                ['ID' => 31]
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
        'mailservice.get',
        { ID: 31 },
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
        'mailservice.get',
        ['ID' => 31]
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
        bitrix_response = client.mailservice.get(
            bitrix_id=31,
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
    "result": {
        "ID": "31",
        "SITE_ID": "s1",
        "ACTIVE": "N",
        "SORT": "600",
        "NAME": "Updated mail service",
        "SERVER": "imap.my2-mail.com",
        "PORT": "993",
        "ENCRYPTION": "Y",
        "LINK": "https://mail.my2-mail.com/",
        "ICON": null,
        "SMTP_SERVER": null,
        "SMTP_PORT": null,
        "SMTP_LOGIN_AS_IMAP": "N",
        "SMTP_PASSWORD_AS_IMAP": "N",
        "SMTP_ENCRYPTION": null,
        "UPLOAD_OUTGOING": null
    },
    "time": {
        "start": 1774009408,
        "finish": 1774009408.234421,
        "duration": 0.2344210147857666,
        "processing": 0,
        "date_start": "2026-03-20T15:23:28+02:00",
        "date_finish": "2026-03-20T15:23:28+02:00",
        "operating_reset_at": 1774010008,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Parameters of the mail service. The structure of the object is described in detail [below](#mail-service) ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result {#mail-service}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the mail service ||
|| **SITE_ID**
[`string`](../data-types.md) | Identifier of the site in Bitrix24 ||
|| **ACTIVE**
[`string`](../data-types.md) | Service activity, values `Y` or `N` ||
|| **SORT**
[`integer`](../data-types.md) | Sort index ||
|| **NAME**
[`string`](../data-types.md) | Name of the service ||
|| **SERVER**
[`string`](../data-types.md) | IMAP server address in lowercase ||
|| **PORT**
[`integer`](../data-types.md) | IMAP server port ||
|| **ENCRYPTION**
[`string`](../data-types.md) | Secure connection indicator, values `Y` or `N` ||
|| **LINK**
[`string`](../data-types.md) | Address of the service's web interface ||
|| **ICON**
[`string`](../data-types.md) | Path to the service's logo image ||
|| **SMTP_SERVER**
[`string`](../data-types.md)\|[`null`](../data-types.md) | SMTP server, if configured ||
|| **SMTP_PORT**
[`integer`](../data-types.md)\|[`null`](../data-types.md) | SMTP port, if configured ||
|| **SMTP_LOGIN_AS_IMAP**
[`string`](../data-types.md) | Use IMAP login for SMTP, values `Y` or `N` ||
|| **SMTP_PASSWORD_AS_IMAP**
[`string`](../data-types.md) | Use IMAP password for SMTP, values `Y` or `N` ||
|| **SMTP_ENCRYPTION**
[`string`](../data-types.md)\|[`null`](../data-types.md) | Secure SMTP connection indicator, values `Y` or `N` ||
|| **UPLOAD_OUTGOING**
[`string`](../data-types.md)\|[`null`](../data-types.md) | Indicator of uploading outgoing emails, values `Y` or `N` ||
|#

## Error Handling

HTTP status: **400**

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
|| `400` | `ERROR_CORE` | Mail service ID not specified | Required parameter `ID` not provided ||
|| `400` | `ERROR_CORE` | Mail service not found | Mail service with the specified `ID` not found ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mailservice-add.md)
- [{#T}](./mailservice-update.md)
- [{#T}](./mailservice-list.md)
- [{#T}](./mailservice-delete.md)
- [{#T}](./mailservice-fields.md)