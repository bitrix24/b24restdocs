# Get the list of mail services mailservice.list

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `mailservice.list` returns a list of active mail services.

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

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod('mailservice.list', {});
    	console.log(response.getData().result);
    }
    catch (error)
    {
    	console.error(error);
    }
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
[`string`](../data-types.md) | Indicator of secure connection, values `Y` or `N` ||
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
[`string`](../data-types.md)\|[`null`](../data-types.md) | Indicator of secure SMTP connection, values `Y` or `N` ||
|| **UPLOAD_OUTGOING**
[`string`](../data-types.md)\|[`null`](../data-types.md) | Indicator of uploading outgoing e-mails, values `Y` or `N` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "No mail services found"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | ERROR_CORE | No mail services found | No active mail services ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mailservice-add.md)
- [{#T}](./mailservice-update.md)
- [{#T}](./mailservice-get.md)
- [{#T}](./mailservice-delete.md)
- [{#T}](./mailservice-fields.md)