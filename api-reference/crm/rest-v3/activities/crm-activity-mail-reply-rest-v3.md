# Reply to an Email v 3.0 crm.activity.mail.reply

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the email and with a mailbox available for sending

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.activity.mail.reply` method sends a reply to a CRM email and saves it as a new email in the same thread with the same bindings to CRM items. The method sends the email to the recipients immediately.

The method determines the recipients and subject itself:

- a reply to an incoming email goes to the sender or to the address from the `Reply-To` header
- a reply to an outgoing email goes to the recipients of the original email
- if the email contains no addresses, the reply goes to the first email address from the communication contacts of the email
- the subject is the subject of the original email with the `Re:` prefix

The sender addresses are excluded from the recipients. The method works only with emails, which are activities with `providerId` equal to `CRM_EMAIL`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **activityId***
[`integer`](../../../data-types.md) | ID of the email to reply to.

The ID can be retrieved with the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method with the `["providerId","=","CRM_EMAIL"]` condition ||
|| **body***
[`string`](../../../data-types.md) | Reply text. An empty string and a string of spaces are not accepted ||
|| **cc**
[`string[]`](../../../data-types.md) | CC addresses ||
|| **bcc**
[`string[]`](../../../data-types.md) | BCC addresses ||
|| **from**
[`string`](../../../data-types.md) | Sender address, for example, `sales@example.com` or `Sales <sales@example.com>`. The address must be among the mailboxes and senders available to the user.

Default: the CRM address from the settings if it is available to the user, otherwise the first available sender ||
|| **senderId**
[`integer`](../../../data-types.md) | Sender ID. Use together with `from`: the address of the sender with this ID must match `from` ||
|| **mailboxId**
[`integer`](../../../data-types.md) | Mailbox ID. Use together with `from`: the mailbox address must match `from` ||
|#

The total number of recipients in `to`, `cc`, and `bcc` is limited by the plan, for example, to 10 addresses. The reply recipient counts toward this number. On the demo plan and on plans without mail synchronization, each field allows only one address.

The method checks the addresses before sending: if `cc` or `bcc` contains an invalid address or the number of recipients exceeds the limit, the email is not sent.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.activity.mail.reply`

{% endnote %}

The examples reply to the email with the ID `747` and copy the manager.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":747,"body":"Hello! Thank you for your email, we will reply by the end of the day.","cc":["manager@example.com"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.mail.reply
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":747,"body":"Hello! Thank you for your email, we will reply by the end of the day.","cc":["manager@example.com"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.activity.mail.reply
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type MailReplyResult = {
      activityId: number
      parentActivityId: number
      from: string
      to: string[]
      warnings: string[]
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<MailReplyResult>(
        'crm.activity.mail.reply',
        {
          activityId: 747,
          body: 'Hello! Thank you for your email, we will reply by the end of the day.',
          cc: ['manager@example.com'],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Reply sent, activity:', result.activityId)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function replyToMail() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.activity.mail.reply',
            {
              activityId: 747,
              body: 'Hello! Thank you for your email, we will reply by the end of the day.',
              cc: ['manager@example.com'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Reply sent, activity:', result.activityId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', replyToMail)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.activity.mail.reply"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "activityId": 747,
                "body": "Hello! Thank you for your email, we will reply by the end of the day.",
                "cc": ["manager@example.com"],
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.mail.reply',
                [
                    'activityId' => 747,
                    'body' => 'Hello! Thank you for your email, we will reply by the end of the day.',
                    'cc' => ['manager@example.com'],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.activity.mail.reply',
        {
            activityId: 747,
            body: 'Hello! Thank you for your email, we will reply by the end of the day.',
            cc: ['manager@example.com'],
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.mail.reply',
        [
            'activityId' => 747,
            'body' => 'Hello! Thank you for your email, we will reply by the end of the day.',
            'cc' => ['manager@example.com'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.mail.reply", b24.Params{
    	"activityId": 747,
    	"body":       "Hello! Thank you for your email, we will reply by the end of the day.",
    	"cc":         []string{"manager@example.com"},
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.mail.reply: %w", err)
    }

    var result struct {
    	ActivityID       int      `json:"activityId"`
    	ParentActivityID int      `json:"parentActivityId"`
    	From             string   `json:"from"`
    	To               []string `json:"to"`
    	Warnings         []string `json:"warnings"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.ActivityID, result.To)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "activityId": 8473,
        "parentActivityId": 8471,
        "from": "sales@example.com",
        "to": [
            "client@example.com"
        ],
        "cc": [],
        "bcc": [],
        "isSyncedToImap": false,
        "warnings": [
            "No mailbox is configured for the chosen \"from\" address; the message is not mirrored to a Sent IMAP folder. Replies are tracked via Reply-To and the URN."
        ]
    },
    "time": {
        "start": 1793712321,
        "finish": 1793712322.461878,
        "duration": 1.4618780612945557,
        "processing": 1,
        "date_start": "2026-11-03T16:25:21+03:00",
        "date_finish": "2026-11-03T16:25:22+03:00",
        "operating_reset_at": 1793712900,
        "operating": 0.46572399139404297
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Sending result ||
|| **result.activityId**
[`integer`](../../../data-types.md) | ID of the new CRM email with the reply ||
|| **result.parentActivityId**
[`integer`](../../../data-types.md) | ID of the email the reply was sent to ||
|| **result.from**
[`string`](../../../data-types.md) | Address the reply was sent from ||
|| **result.to**
[`string[]`](../../../data-types.md) | Reply recipients ||
|| **result.cc**
[`string[]`](../../../data-types.md) | CC addresses ||
|| **result.bcc**
[`string[]`](../../../data-types.md) | BCC addresses ||
|| **result.isSyncedToImap**
[`boolean`](../../../data-types.md) | `true` if the email was saved to the Sent folder of the mailbox. `false` if no mailbox is connected for the sender address: the email was sent but not saved to the Sent folder ||
|| **result.warnings**
[`string[]`](../../../data-types.md) | Warnings that did not prevent sending ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "CRM_EMAIL_INVALID_REQUEST",
        "message": "body must be a non-empty string."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `CRM_EMAIL_INVALID_REQUEST` | activityId must be a positive integer. | Pass `activityId` as a positive integer. The method also checks `senderId` and `mailboxId`. HTTP status: **400** ||
|| `CRM_EMAIL_INVALID_REQUEST` | body must be a non-empty string. | Pass the reply text in `body`. HTTP status: **400** ||
|| `CRM_EMAIL_INVALID_REQUEST` | Parent activity is not a CRM email (PROVIDER_ID must be "CRM_EMAIL"). | Pass the ID of an email, not of an activity of another kind. HTTP status: **400** ||
|| `CRM_EMAIL_INVALID_REQUEST` | Cannot determine reply recipients from the parent activity. | The original email has no addresses to reply to. Send the email with the previous version methods, specifying the recipients explicitly. HTTP status: **400** ||
|| `CRM_EMAIL_ACTIVITY_NOT_FOUND` | Parent CRM email activity not found. | The email does not exist, or the email's client is hidden from you by access settings. HTTP status: **404** ||
|| `CRM_EMAIL_ACCESS_DENIED` | Access denied to the parent CRM email activity. | You have no permission to view the email. HTTP status: **403** ||
|| `CRM_EMAIL_INVALID_RECIPIENT` | Invalid recipient email(s): `#EMAILS#` | Check the addresses in `cc` and `bcc`. HTTP status: **400** ||
|| `CRM_EMAIL_TOO_MANY_RECIPIENTS` | Too many recipients: `#COUNT#`, the limit is `#LIMIT#` total across to/cc/bcc. | Reduce the number of addresses in `cc` and `bcc`. The limit is given in the error message, and the reply recipient counts toward it. HTTP status: **400** ||
|| `CRM_EMAIL_SEND_FAILED` | Tariff restriction: cannot send to more than `#LIMIT#` recipient(s) per to/cc/bcc field. | The plan limits the number of addresses in a single field. Reduce the number of addresses in `cc` or `bcc`. HTTP status: **400** ||
|| `CRM_EMAIL_RECIPIENT_BLACKLISTED` | Some recipients are in the global mail blacklist: `#EMAILS#` | Remove the addresses from the mail blacklist. HTTP status: **400** ||
|| `CRM_EMAIL_SENDER_NOT_AVAILABLE` | No available email sender is configured for this user. | Connect a mailbox or a sender for the user. HTTP status: **400** ||
|| `CRM_EMAIL_SENDER_NOT_AVAILABLE` | Sender email is not available for this user. | The address from `from` or the mailbox from `senderId` or `mailboxId` is not available to the user. Pass an available address. HTTP status: **400** ||
|| `CRM_EMAIL_INVALID_FROM` | Cannot resolve a valid "from" address. | Check the address in `from`. HTTP status: **400** ||
|| `CRM_EMAIL_MAIL_MODULE_UNAVAILABLE` | The "mail" module is not available. | Mail is not available on this Bitrix24. HTTP status: **400** ||
|| `CRM_EMAIL_SEND_FAILED` | `#MESSAGE#` | The email was not sent. Retry the request later. HTTP status: **400** ||
|| `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION` | This request requires data type `int` in field `activityId` | Pass the parameter of the type specified in its description. HTTP status: **400** ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-mail-get-content-rest-v3.md)
- [{#T}](./crm-activity-mail-get-thread-rest-v3.md)
