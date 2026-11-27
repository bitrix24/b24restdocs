# Reply to an Email v 3.0 crm.activity.email.reply

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the email and permission to edit the CRM item it is bound to. The user must have an available mailbox or sender

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

Sends a reply to a CRM email and retains it as a new email in the same thread with the same CRM item bindings. Sends the email to the recipients immediately and returns the new email in the same format as the [crm.activity.email.get](./crm-activity-email-get-rest-v3.md) method. Replaces the deprecated `crm.activity.mail.reply` method.

The method determines the recipients and subject automatically:

- a reply to an incoming email is sent to the address in the `Reply-To` header or to the sender
- a reply to an outgoing email is sent to the original recipients
- if the email contains no addresses, the reply is sent to the first email address in the email's communication contacts
- the subject is the original email's subject prefixed with `Re:`

Mailbox addresses used in the correspondence are excluded from the recipients. The method works only with emails: activities with `providerId` equal to `CRM_EMAIL`.

The method supports the `Idempotency-Key` header: repeating a request with the same key returns the first response without sending the email again. For details, see [Repeat Calls without Duplicates](../../../rest-v3.md#idempotency).

The method name is written in lowercase: `crm.activity.email.reply`. A name with different capitalization returns the [Method Not Found](#method-not-found) error.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | ID of the email to reply to.

The ID can be retrieved with the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method using the `["providerId","=","CRM_EMAIL"]` condition ||
|| **fields***
[`object`](../../../data-types.md) | Reply fields. [Detailed Description](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **body***
[`string`](../../../data-types.md) | Reply text. Empty strings and whitespace-only strings are not accepted ||
|| **cc**
[`string[]`](../../../data-types.md) | CC addresses ||
|| **bcc**
[`string[]`](../../../data-types.md) | BCC addresses ||
|| **from**
[`string`](../../../data-types.md) | Sender address, for example, `sales@example.com` or `Sales Department <sales@example.com>`. The address must belong to a mailbox or sender available to the user.

Default: the CRM address from the settings if it is available to the user; otherwise, the first available sender. An empty string is equivalent to omitting `from` ||
|| **senderId**
[`integer`](../../../data-types.md) | Sender ID. Use with `from`: the address of the sender with this ID must match `from` ||
|| **mailboxId**
[`integer`](../../../data-types.md) | Mailbox ID. Use with `from`: the mailbox address must match `from` ||
|#

No other fields are accepted in `fields`: the recipients `to`, subject `subject`, and reply bindings are taken from the original email. Any other field results in a validation error.

The total number of recipients in `to`, `cc`, and `bcc` is limited, including the reply recipient. The plan may also limit the number of addresses per field. The method removes addresses from `cc` that are already among the recipients and addresses from `bcc` that are among the recipients or in `cc`.

The method validates the addresses and sender before sending: if `cc` or `bcc` contains an invalid address, the recipient limit is exceeded, or the sender is unavailable, the email is not sent.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.activity.email.reply`

{% endnote %}

The examples reply to the email with ID `8691`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8691,"fields":{"body":"Thank you for your email! We will reply by the end of the day.","from":"manager@example.com"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.email.reply
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8691,"fields":{"body":"Thank you for your email! We will reply by the end of the day.","from":"manager@example.com"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.activity.email.reply
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EmailReplyResult = {
      item: {
        id: number
        subject: string
        from: string | null
        to: string[] | null
        parentActivityId: number | null
        body: string | null
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<EmailReplyResult>(
        'crm.activity.email.reply',
        {
          id: 8691,
          fields: {
            body: 'Thank you for your email! We will reply by the end of the day.',
            from: 'manager@example.com',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Reply sent:', result.item.id, result.item.to)
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
      async function replyToEmail() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.activity.email.reply',
            {
              id: 8691,
              fields: {
                body: 'Thank you for your email! We will reply by the end of the day.',
                from: 'manager@example.com',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Reply sent:', result.item.id, result.item.to)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', replyToEmail)
    </script>
    ```

- Python

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.activity.email.reply"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 8691,
                "fields": {
                    "body": "Thank you for your email! We will reply by the end of the day.",
                    "from": "manager@example.com",
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["item"])
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, through cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.email.reply',
                [
                    'id' => 8691,
                    'fields' => [
                        'body' => 'Thank you for your email! We will reply by the end of the day.',
                        'from' => 'manager@example.com',
                    ],
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, through cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.activity.email.reply',
        {
            id: 8691,
            fields: {
                body: 'Thank you for your email! We will reply by the end of the day.',
                from: 'manager@example.com',
            },
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, through cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.email.reply',
        [
            'id' => 8691,
            'fields' => [
                'body' => 'Thank you for your email! We will reply by the end of the day.',
                'from' => 'manager@example.com',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx have already been created — see the "SDK for Go" section
    res, err := client.Core().Call(ctx, "crm.activity.email.reply", b24.Params{
    	"id": 8691,
    	"fields": map[string]any{
    		"body": "Thank you for your email! We will reply by the end of the day.",
    		"from": "manager@example.com",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.activity.email.reply: %w", err)
    }

    var result struct {
    	Item struct {
    		ID               int      `json:"id"`
    		Subject          string   `json:"subject"`
    		From             *string  `json:"from"`
    		To               []string `json:"to"`
    		ParentActivityID *int     `json:"parentActivityId"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(result.Item.ID, result.Item.Subject, result.Item.To)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "subject": "Re: Delivery Inquiry",
            "direction": "outgoing",
            "communications": [
                {
                    "type": "EMAIL",
                    "value": "client@example.com",
                    "entityTypeId": 2,
                    "entityId": 741
                }
            ],
            "from": "manager@example.com",
            "to": [
                "client@example.com"
            ],
            "cc": [],
            "bcc": [],
            "parentActivityId": 8691,
            "body": "Thank you for your email! We will reply by the end of the day.\n\nSent from Bitrix24",
            "isBodyTruncated": false,
            "id": 8697,
            "providerId": "CRM_EMAIL",
            "isCompleted": true,
            "assignedById": 1,
            "authorId": 1,
            "editorId": 1,
            "createdTime": "2026-11-27T12:41:56+03:00",
            "updatedTime": "2026-11-27T12:41:58+03:00",
            "startTime": "2026-11-27T12:41:56+03:00",
            "endTime": "2026-11-27T12:41:56+03:00",
            "deadlineTime": "2026-11-27T12:41:56+03:00",
            "bindings": [
                {
                    "entityTypeId": 2,
                    "entityId": 741
                },
                {
                    "entityTypeId": 3,
                    "entityId": 71
                }
            ]
        }
    },
    "time": {
        "start": 1795772516,
        "finish": 1795772518.192819,
        "duration": 2.192819118499756,
        "processing": 2,
        "date_start": "2026-11-27T12:41:56+03:00",
        "date_finish": "2026-11-27T12:41:58+03:00",
        "operating_reset_at": 1795773060,
        "operating": 3.965543031692505
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Response data object ||
|| **result.item**
[`object`](../../../data-types.md) | The new reply email with all fields, as in the crm.activity.email.get response. `parentActivityId` contains the ID of the email being replied to. [Email Field Descriptions](./crm-activity-email-get-rest-v3.md#fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "CRM_EMAIL_SENDER_NOT_AVAILABLE",
        "message": "Sender email is not available for this user."
    }
}
```

{% include notitle [Error Handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Required field `id` is not specified | Pass the email `id` ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|| `id` | — | Pass a positive ID ||
|| `fields` | Required field `fields` is not specified | Pass a `fields` object with the reply text ||
|| `body` | body is required. | Pass the reply text in `fields.body` ||
|| `body` | body must be a non-empty string. | Pass non-empty text in `body` ||
|| `#FIELD#` | `#FIELD#` is not accepted by a reply: the answer goes to the thread of the letter. | Remove the field from `fields`. A reply accepts only `body`, `cc`, `bcc`, `from`, `senderId`, and `mailboxId` ||
|| `cc.#INDEX#`, `bcc.#INDEX#` | `#FIELD#` must contain email addresses only. | Remove empty strings from `cc` and `bcc` ||
|| `cc`, `bcc` | This request requires data type `array` in field `#FIELD#` | Pass the addresses as an array of strings, even for a single address ||
|| `senderId`, `mailboxId` | This request requires data type `int` in field `#FIELD#` | Pass the ID as a number ||
|#

#### Field Validation Errors

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `cc`, `bcc` | — | Pass the addresses as strings ||
|| `senderId`, `mailboxId` | — | Pass a positive ID ||
|#

#### Unknown Field Error

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Pass `fields` as an object with field names, rather than a string or array ||
|#

#### Email Not Found

Error code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | The email does not exist, the activity with this ID is not an email, you do not have permission to view the email, or its customer is hidden from you by access settings. Check `id` with the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method using the `["providerId","=","CRM_EMAIL"]` condition ||
|#

#### Sending Errors

Error codes `CRM_EMAIL_*` are returned if the email could not be sent. The HTTP status is **400** unless otherwise stated in the description.

#|
|| **Code** | **Error Description** | **How to Fix** ||
|| `CRM_EMAIL_INVALID_RECIPIENT` | Invalid recipient email(s): `#EMAILS#` | Check the addresses in `cc` and `bcc` ||
|| `CRM_EMAIL_TOO_MANY_RECIPIENTS` | Too many recipients: `#COUNT#`, the limit is `#LIMIT#` total across to/cc/bcc. | Reduce the number of addresses in `cc` and `bcc`. The limit is specified in the error message and includes the reply recipient ||
|| `CRM_EMAIL_SEND_FAILED` | Tariff restriction: cannot send to more than `#LIMIT#` recipient(s) per to/cc/bcc field. | The plan limits the number of addresses per field. Reduce the number of addresses in `cc` or `bcc` ||
|| `CRM_EMAIL_RECIPIENT_BLACKLISTED` | Some recipients are in the global mail blacklist: `#EMAILS#` | Remove the addresses from the mail blacklist ||
|| `CRM_EMAIL_SENDER_NOT_AVAILABLE` | No available email sender is configured for this user. | Connect a mailbox for the user or add a sender ||
|| `CRM_EMAIL_SENDER_NOT_AVAILABLE` | Sender email is not available for this user. | The address in `from` is unavailable to the user, or the mailbox `mailboxId` or sender `senderId` does not match `from`. Pass an available address, and use `mailboxId` and `senderId` only with `from` ||
|| `CRM_EMAIL_INVALID_FROM` | Sender email is not available for this user. | The value in `from` is not an email address. Pass an address, for example, `sales@example.com` ||
|| `CRM_EMAIL_INVALID_FROM` | Cannot resolve a valid "from" address. | The selected sender has an invalid address. Pass another available address in `from` ||
|| `CRM_EMAIL_INVALID_REQUEST` | Cannot determine reply recipients from the parent activity. | The original email has no reply addresses. Send an email using `crm.<type>.activity.email.send` with explicit recipients ||
|| `CRM_EMAIL_INVALID_REQUEST` | Parent activity has no main owner. | The original email has no main CRM item. You cannot reply to such an email ||
|| `CRM_EMAIL_ACCESS_DENIED` | Access denied to create CRM email activity on the main owner. | No permission to edit the CRM item the email is bound to. HTTP status **403** ||
|| `CRM_EMAIL_MAIL_MODULE_UNAVAILABLE` | The "mail" module is not available. | Mail is unavailable in this Bitrix24 ||
|| `CRM_EMAIL_TOO_MANY_RECIPIENTS` | Bitrix24 mail-sending limit reached for this portal. | The Bitrix24 email sending limit has been reached. Try again later ||
|| `CRM_EMAIL_SEND_FAILED` | `#MESSAGE#` | The email was not sent. Try again later ||
|#

#### Method Not Found {#method-not-found}

Error code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Check the method name: it is written in lowercase, `crm.activity.email.reply` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Exploring

- [{#T}](./crm-activity-email-get-rest-v3.md)
- [{#T}](./crm-activity-email-get-thread-rest-v3.md)
- [{#T}](./crm-entity-activity-email-list-rest-v3.md)
- [{#T}](./index.md)
