# Get the list of mail services mailservice.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `mailservice.list` returns a list of active mail services of the current site.

## Method Parameters

No parameters.

The method returns all active services in a single array sorted by the `SORT` field and, for equal values, by `NAME`. There is no pagination, and the `start` parameter is not used. Inactive services are not included in the list; you can retrieve them by `ID` using the [mailservice.get](./mailservice-get.md) method

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/mailservice.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth": "**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/mailservice.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each mail service item returned in result[]
    type MailServiceItem = {
      ID: string
      SITE_ID: string
      ACTIVE: string
      SORT: string
      NAME: string
      SERVER: string
      PORT: string | null
      ENCRYPTION: string | null
      LINK: string | null
      ICON: string | null
      SMTP_SERVER: string | null
      SMTP_PORT: string | null
      SMTP_LOGIN_AS_IMAP: string
      SMTP_PASSWORD_AS_IMAP: string
      SMTP_ENCRYPTION: string | null
      UPLOAD_OUTGOING: string | null
    }

    try {
      // mailservice.list returns all active services of the current site at once, without pagination
      const response = await $b24.actions.v2.call.make<MailServiceItem[]>({
        method: 'mailservice.list',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Mail services:', result.length, result)
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
      async function listMailServices() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // mailservice.list returns all active services of the current site at once, without pagination
          const response = await $b24.actions.v2.call.make({
            method: 'mailservice.list',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Mail services:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listMailServices)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.mailservice.list().response
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call('mailservice.list', []);

        $result = $response->getResponseData()->getResult();
        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'mailservice.list',
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

    $result = CRest::call('mailservice.list', []);
    print_r($result);
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "mailservice.list", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("mailservice.list: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "1",
            "SITE_ID": "s1",
            "ACTIVE": "Y",
            "SORT": "100",
            "NAME": "gmail",
            "SERVER": "imap.gmail.com",
            "PORT": "993",
            "ENCRYPTION": "Y",
            "LINK": "https://mail.google.com/",
            "ICON": "https://cdn.com.bitrix24.com/b17053/mail/mailservices/icon/ac0/ac08d334f35100d98c1a628f4f57f25c/post_gmail_icon.png",
            "SMTP_SERVER": "smtp.gmail.com",
            "SMTP_PORT": "465",
            "SMTP_LOGIN_AS_IMAP": "Y",
            "SMTP_PASSWORD_AS_IMAP": "Y",
            "SMTP_ENCRYPTION": "Y",
            "UPLOAD_OUTGOING": "N"
        },
        ... // description for each mail service
    ],
    "time": {
        "start": 1774009798,
        "finish": 1774009798.248488,
        "duration": 0.2484879493713379,
        "processing": 0,
        "date_start": "2026-03-20T15:29:58+02:00",
        "date_finish": "2026-03-20T15:29:58+02:00",
        "operating_reset_at": 1774010398,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../data-types.md) | Array of objects with information about mail services.

The structure of the mail service object is described in detail [below](#mail-services) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Mailservice Object {#mail-services}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Identifier of the mail service, for example, `"31"` ||
|| **SITE_ID**
[`string`](../data-types.md) | Identifier of the site in Bitrix24 ||
|| **ACTIVE**
[`string`](../data-types.md) | Service activity, values `Y` or `N` ||
|| **SORT**
[`string`](../data-types.md) | Sort index, for example, `"100"` ||
|| **NAME**
[`string`](../data-types.md) | Name of the service ||
|| **SERVER**
[`string`](../data-types.md) | IMAP server address in lowercase. For the built-in `bitrix24` service, it is an empty string ||
|| **PORT**
[`string`](../data-types.md)\|[`null`](../data-types.md) | IMAP server port, for example, `"993"`. `null` if the port is not set ||
|| **ENCRYPTION**
[`string`](../data-types.md)\|[`null`](../data-types.md) | Indicator of secure connection, values `Y` or `N`. `null` if the indicator is not set ||
|| **LINK**
[`string`](../data-types.md)\|[`null`](../data-types.md) | Address of the service's web interface. `null` if the address is not set ||
|| **ICON**
[`string`](../data-types.md)\|[`null`](../data-types.md) | Address of the service's logo. For an uploaded logo, it is the full URL of the file; for a built-in service, it is a path from the Bitrix24 root, for example, `/bitrix/images/mail/mailservice-icon/post-gmail-icon.svg`. `null` if the logo is not uploaded and the service name does not match a built-in one ||
|| **SMTP_SERVER**
[`string`](../data-types.md)\|[`null`](../data-types.md) | SMTP server, if configured ||
|| **SMTP_PORT**
[`string`](../data-types.md)\|[`null`](../data-types.md) | SMTP port, if configured ||
|| **SMTP_LOGIN_AS_IMAP**
[`string`](../data-types.md) | Use IMAP login for SMTP, values `Y` or `N` ||
|| **SMTP_PASSWORD_AS_IMAP**
[`string`](../data-types.md) | Use IMAP password for SMTP, values `Y` or `N` ||
|| **SMTP_ENCRYPTION**
[`string`](../data-types.md)\|[`null`](../data-types.md) | Indicator of secure SMTP connection, values `Y` or `N` ||
|| **UPLOAD_OUTGOING**
[`string`](../data-types.md)\|[`null`](../data-types.md) | Indicator of uploading outgoing e-mails, values `Y` or `N` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "E-mail services were not found."
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CORE` | E-mail services were not found | The current site has no active mail services. The method returns an error instead of an empty array ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./mailservice-add.md)
- [{#T}](./mailservice-update.md)
- [{#T}](./mailservice-get.md)
- [{#T}](./mailservice-delete.md)
- [{#T}](./mailservice-fields.md)