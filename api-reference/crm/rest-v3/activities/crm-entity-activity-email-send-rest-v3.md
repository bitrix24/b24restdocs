# Send an Email v 3.0 crm.[type].activity.email.send

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM item and an available mailbox or sender address

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

Sends an email from a CRM item and retains it as an activity of that item. Sends the email to the recipients immediately and returns it in the same format as the [crm.&lt;type&gt;.activity.email.get](./crm-entity-activity-email-get-rest-v3.md) method.

The sent email:

- is retained as a completed outgoing activity, with the user who called the method as the responsible person
- is bound to the item specified in `ownerId`
- links each recipient address to a CRM item: the item's contact or company if they have this address, or otherwise the item itself. The links are returned in `communications`

The method supports the `Idempotency-Key` header: repeating a request with the same key returns the same email without sending it again. For details, see [Repeat Calls without Duplicates](../../../rest-v3.md#idempotency).

To reply to an email in the same thread, use the [crm.activity.email.reply](./crm-activity-email-reply-rest-v3.md) method.

## Method Name for the Object Type

#|
|| **Object Type** | **Method** ||
|| Deal | `crm.deal.activity.email.send` ||
|| Lead | `crm.lead.activity.email.send` ||
|| Contact | `crm.contact.activity.email.send` ||
|| Company | `crm.company.activity.email.send` ||
|| Estimate | `crm.quote.activity.email.send` ||
|| Invoice | `crm.smartinvoice.activity.email.send` ||
|| CRM Document | `crm.smartdocument.activity.email.send` ||
|| SPA | `crm.smartprocess<entityTypeId>.activity.email.send`, for example `crm.smartprocess1038.activity.email.send` ||
|#

For an SPA, substitute its type ID `entityTypeId` in the method name. SPA type IDs `entityTypeId` are returned by the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method.

The method name is written in lowercase: `crm.smartinvoice.activity.email.send`, rather than `crm.smartInvoice.activity.Email.send`. A name with different capitalization returns the [Method Not Found](#method-not-found) error.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Email fields. Required fields are `ownerId`, `subject`, `to`, and `body`. [Parameter Description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ownerId***
[`integer`](../../../data-types.md) | ID of the CRM item the email is sent from. The item must belong to the object type specified in the method name. This field is not returned in the response.

The ID can be retrieved with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| **subject***
[`string`](../../../data-types.md) | Email subject. If an empty string is passed, Bitrix24 takes the subject from the beginning of the email text, or uses `(no subject)` if it cannot ||
|| **to***
[`string[]`](../../../data-types.md) | Recipient addresses, at least one. Pass an address as a string: `client@example.com` or `Name <client@example.com>`. The name is not included in the email, and the address is retained in lowercase ||
|| **body***
[`string`](../../../data-types.md) | Email body: plain text or HTML. Empty strings and whitespace-only strings are not accepted. Unsafe HTML markup is removed, and a signature is added if enabled in the mail settings ||
|| **cc**
[`string[]`](../../../data-types.md) | CC addresses. Addresses already in `to` are removed from CC ||
|| **bcc**
[`string[]`](../../../data-types.md) | BCC addresses. Addresses already in `to` or `cc` are removed from BCC ||
|| **from**
[`string`](../../../data-types.md) | Sender address, for example, `sales@example.com` or `Sales Department <sales@example.com>`. The address must be among the mailboxes and sender addresses available to the user.

Default: the CRM address from the settings if it is available to the user; otherwise, the first available address ||
|| **senderId**
[`integer`](../../../data-types.md) | ID of a sender address verified without connecting a mailbox. Use with `from`: the sender address must match `from`. This field is not returned in the response ||
|| **mailboxId**
[`integer`](../../../data-types.md) | ID of the connected mailbox. Use with `from`: the mailbox address must match `from`. This field is not returned in the response ||
|#

Usually, `from` is sufficient: the method finds the mailbox or sender with this address automatically. The `senderId` and `mailboxId` parameters are needed only to select a specific mailbox or sender when the same address is available to the user in several ways. These parameters do not work without `from`: the method returns `CRM_EMAIL_SENDER_NOT_AVAILABLE`.

If the address in `from` does not belong to a connected mailbox, the email is still sent, but a copy does not appear in the mailbox's sent folder.

The method validates addresses before sending. If any address is invalid or on the mail blacklist, or the recipient limit is exceeded, the email is not sent and no activity is created. The total number of addresses in `to`, `cc`, and `bcc` is limited; the limit is specified in the error message. The plan may also limit the number of addresses in each field.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.deal.activity.email.send`

{% endnote %}

The examples send an email to a customer from a deal. For another object type, replace the method name using the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -H "Idempotency-Key: 7f2d3d50-9235-4e56-b768-b8a78c58e4f2" \
    -d '{"fields":{"ownerId":741,"subject":"Sales Proposal","to":["client@example.com"],"body":"Hello! Here is our sales proposal.","from":"sales@example.com"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.activity.email.send
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -H "Idempotency-Key: 7f2d3d50-9235-4e56-b768-b8a78c58e4f2" \
    -d '{"fields":{"ownerId":741,"subject":"Sales Proposal","to":["client@example.com"],"body":"Hello! Here is our sales proposal.","from":"sales@example.com"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.deal.activity.email.send
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EmailSendResult = {
      item: {
        id: number
        subject: string
        from: string | null
        to: string[] | null
        createdTime: string
        communications: Array<{ type: string; value: string | null; entityTypeId: number | null; entityId: number | null }>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<EmailSendResult>(
        'crm.deal.activity.email.send',
        {
          fields: {
            ownerId: 741,
            subject: 'Sales Proposal',
            to: ['client@example.com'],
            body: 'Hello! Here is our sales proposal.',
            from: 'sales@example.com',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Email sent:', result.item.id)
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
      async function sendEmail() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.deal.activity.email.send',
            {
              fields: {
                ownerId: 741,
                subject: 'Sales Proposal',
                to: ['client@example.com'],
                body: 'Hello! Here is our sales proposal.',
                from: 'sales@example.com',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Email sent:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', sendEmail)
    </script>
    ```

- Python

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.deal.activity.email.send"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "ownerId": 741,
                    "subject": "Sales Proposal",
                    "to": ["client@example.com"],
                    "body": "Hello! Here is our sales proposal.",
                    "from": "sales@example.com",
                },
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, through cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.activity.email.send',
                [
                    'fields' => [
                        'ownerId' => 741,
                        'subject' => 'Sales Proposal',
                        'to' => ['client@example.com'],
                        'body' => 'Hello! Here is our sales proposal.',
                        'from' => 'sales@example.com',
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
        'crm.deal.activity.email.send',
        {
            fields: {
                ownerId: 741,
                subject: 'Sales Proposal',
                to: ['client@example.com'],
                body: 'Hello! Here is our sales proposal.',
                from: 'sales@example.com',
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
        'crm.deal.activity.email.send',
        [
            'fields' => [
                'ownerId' => 741,
                'subject' => 'Sales Proposal',
                'to' => ['client@example.com'],
                'body' => 'Hello! Here is our sales proposal.',
                'from' => 'sales@example.com',
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
    res, err := client.Core().Call(ctx, "crm.deal.activity.email.send", b24.Params{
    	"fields": b24.Params{
    		"ownerId": 741,
    		"subject": "Sales Proposal",
    		"to":      []string{"client@example.com"},
    		"body":    "Hello! Here is our sales proposal.",
    		"from":    "sales@example.com",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.deal.activity.email.send: %w", err)
    }

    var result struct {
    	Item struct {
    		ID          int      `json:"id"`
    		Subject     string   `json:"subject"`
    		From        *string  `json:"from"`
    		To          []string `json:"to"`
    		CreatedTime string   `json:"createdTime"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(result.Item.ID, result.Item.Subject)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "subject": "Sales Proposal",
            "direction": "outgoing",
            "communications": [
                {
                    "type": "EMAIL",
                    "value": "client@example.com",
                    "entityTypeId": 2,
                    "entityId": 741
                }
            ],
            "from": "sales@example.com",
            "to": [
                "client@example.com"
            ],
            "cc": [],
            "bcc": [],
            "parentActivityId": null,
            "body": "Hello! Here is our sales proposal.\n\nSent from Bitrix24",
            "isBodyTruncated": false,
            "id": 8695,
            "providerId": "CRM_EMAIL",
            "isCompleted": true,
            "assignedById": 1,
            "authorId": 1,
            "editorId": 1,
            "createdTime": "2026-11-27T12:41:49+03:00",
            "updatedTime": "2026-11-27T12:41:50+03:00",
            "startTime": "2026-11-27T12:41:49+03:00",
            "endTime": "2026-11-27T12:41:49+03:00",
            "deadlineTime": "2026-11-27T12:41:49+03:00",
            "bindings": [
                {
                    "entityTypeId": 2,
                    "entityId": 741
                }
            ]
        }
    },
    "time": {
        "start": 1795772508,
        "finish": 1795772510.740127,
        "duration": 2.7401270866394043,
        "processing": 1,
        "date_start": "2026-11-27T12:41:48+03:00",
        "date_finish": "2026-11-27T12:41:50+03:00",
        "operating_reset_at": 1795773060,
        "operating": 1.3997678756713867
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
[`object`](../../../data-types.md) | The sent email with all fields, including the email text in `body`. The fields are the same as in the [Item Object](./crm-entity-activity-email-get-rest-v3.md#item) of the `crm.<type>.activity.email.get` method. The `ownerId`, `senderId`, and `mailboxId` fields are not returned: the email's item is returned in `bindings` ||
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
|| `fields` | Required field `fields` is not specified | Pass the email fields in `fields` ||
|| `to` | to must contain at least one email address. | Pass at least one address in `to` ||
|| `to.0` | to must contain email addresses only. | Remove empty strings from `to`. The method validates `cc` and `bcc` in the same way ||
|| `to` | This request requires data type `array` in field `to` | Pass the addresses as an array, even for a single address: `["client@example.com"]`. The method validates `cc` and `bcc` in the same way ||
|| `body` | body must be a non-empty string. | Pass the email text in `body` ||
|| `ownerId` | This request requires data type `int` in field `ownerId` | Pass the item ID as a number ||
|#

#### Field Validation Errors

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `subject` | — | Pass the email subject. The method validates `to`, `body`, and `ownerId` in the same way ||
|| `ownerId` | — | Pass a positive item ID. The method validates `senderId` and `mailboxId` in the same way ||
|| `to` | — | Pass the addresses as strings ||
|| `id` | — | Remove fields from `fields` that are not in the [Parameter fields](#fields) table ||
|#

#### Unknown Field Error

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Check the field names in `fields` against the [Parameter fields](#fields) table ||
|#

#### Item Not Found

Error code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | The item from `ownerId` does not exist for this object type, or you do not have permission to edit it. Check `ownerId` and the method name ||
|#

#### Invalid Recipient Address

Error code: `CRM_EMAIL_INVALID_RECIPIENT`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Invalid recipient email(s): to: not-an-email | Correct the addresses listed in the error message. The method validates `to`, `cc`, and `bcc` ||
|#

#### Blacklisted Address

Error code: `CRM_EMAIL_RECIPIENT_BLACKLISTED`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Some recipients are in the global mail blacklist: `#EMAILS#` | Remove addresses on the mail blacklist from the recipients ||
|#

#### Limit Exceeded

Error code: `CRM_EMAIL_TOO_MANY_RECIPIENTS`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Too many recipients: `#COUNT#`, the limit is `#LIMIT#` total across to/cc/bcc. | Reduce the total number of addresses in `to`, `cc`, and `bcc` to the limit in the error message, or send several emails ||
|| `-` | Bitrix24 mail-sending limit reached for this portal. | The Bitrix24 email sending limit has been reached. Try again later ||
|| `-` | Bitrix24 custom mail-sending limit reached for this portal. | The email sending limit set by the Bitrix24 administrator has been reached. Try again later ||
|#

#### Sender Unavailable

Error code: `CRM_EMAIL_SENDER_NOT_AVAILABLE`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | No available email sender is configured for this user. | The user has no mailbox or sender address. Connect a mailbox in Bitrix24 ||
|| `-` | Sender email is not available for this user. | The address in `from` is unavailable to the user, or `senderId` or `mailboxId` does not match `from` or was passed without `from`. Pass an available address in `from` ||
|#

#### Invalid Sender Address

Error code: `CRM_EMAIL_INVALID_FROM`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Sender email is not available for this user. | The value of `from` was not recognized as an address. Pass an address in the form `sales@example.com` or `Sales Department <sales@example.com>` ||
|| `-` | Cannot resolve a valid "from" address. | The selected sender's address is invalid. Pass another available address in `from` ||
|#

#### Email Not Sent

Error code: `CRM_EMAIL_SEND_FAILED`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Tariff restriction: cannot send to more than `#LIMIT#` recipient(s) per to/cc/bcc field. | The plan limits the number of addresses per field. Reduce the number of addresses in `to`, `cc`, or `bcc` ||
|| `-` | Failed to send the email through the Bitrix mail system. | The email was not sent, and its activity was deleted. Try again later ||
|#

#### Mail Unavailable

Error code: `CRM_EMAIL_MAIL_MODULE_UNAVAILABLE`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | The "mail" module is not available. | Mail is unavailable in this Bitrix24 ||
|#

#### Idempotency Key Reused

Error code: `BITRIX_REST_V3_EXCEPTION_IDEMPOTENCYKEYREUSEDEXCEPTION`, HTTP status **422**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Pass a new `Idempotency-Key` for a new email. Use the previous key only when repeating the same request ||
|#

#### Access Error

Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | For `crm.lead.activity.email.send`: leads are disabled in Bitrix24. Enable leads or send the email from an item of another object type ||
|#

#### Method Not Found {#method-not-found}

Error code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Check the method name: it is written in lowercase, for example, `crm.deal.activity.email.send`. For an SPA, check `entityTypeId` with the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method. My company and B2E documents have no email methods ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](./crm-entity-activity-email-get-rest-v3.md)
- [{#T}](./crm-entity-activity-email-list-rest-v3.md)
- [{#T}](./crm-activity-email-get-rest-v3.md)
- [{#T}](./crm-activity-email-reply-rest-v3.md)
