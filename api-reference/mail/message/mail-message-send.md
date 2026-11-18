# Send an e-mail mail.message.send

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`mail`](../../scopes/permissions.md)
>
> Who can execute the method: a user who has access to the sender address passed in `from`

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `mail.message.send` method sends a new e-mail on behalf of a sender available to the user.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **from***
[`string`](../../data-types.md) | Sender address. The address must be in the list of senders available to the user.

You can pass the address as `Name <email>`: the method uses only the address.

You can retrieve available addresses using the [mail.mailbox.senders](../mailbox/mail-mailbox-senders.md) method, field `email` ||
|| **to***
[`array`](../../data-types.md) | A non-empty array of recipients. Each element is an e-mail address or a recipient name.

The method looks up a name first in the user's mail address book and then among employees. If several matches or no matches are found, the method returns the `RESOLVE_RECIPIENTS_ERROR` error. To avoid depending on the lookup, pass e-mail addresses.

You can retrieve address book addresses using the [mail.recipient.listContacts](../recipient/mail-recipient-listcontacts.md) method, and employee addresses using the [mail.recipient.listEmployees](../recipient/mail-recipient-listemployees.md) method ||
|| **subject***
[`string`](../../data-types.md) | E-mail subject. An empty string is not allowed ||
|| **body***
[`string`](../../data-types.md) | E-mail body. An empty string is not allowed.

You can pass plain text or HTML. The e-mail is sent in HTML format ||
|| **cc**
[`array`](../../data-types.md) | Array of CC recipients. Elements have the same format as in `to`.

Pass an array, even for a single address ||
|| **bcc**
[`array`](../../data-types.md) | Array of BCC recipients. Elements have the same format as in `to`.

Pass an array, even for a single address ||
|| **mailboxId**
[`integer`](../../data-types.md) | ID of the mailbox to send the e-mail through. Required if the `from` address is available to the user through several mailboxes or senders. If both `mailboxId` and `senderId` are passed, the method uses `mailboxId`.

The value must be a positive number and match the `from` address.

You can retrieve the ID using the [mail.mailbox.senders](../mailbox/mail-mailbox-senders.md) method, field `mailboxId` ||
|| **senderId**
[`integer`](../../data-types.md) | Sender ID. Use it if the `from` address is available to the user through several senders.

The value must be a positive number and match the `from` address.

You can retrieve the ID using the [mail.mailbox.senders](../mailbox/mail-mailbox-senders.md) method, field `senderId` ||
|#

If the `from` address belongs to a mailbox connected by the user, the e-mail is sent through that mailbox and saved to its Sent folder. If the address belongs to a standalone sender without a mailbox, the e-mail is sent through the Bitrix24 mail service.

{% note warning "" %}

Before sending, the method checks the Bitrix24 limits:

- the total number of recipients in `to`, `cc`, and `bcc` is limited by the plan. If no limit is set, up to 10 recipients are allowed
- on the trial plan and on plans without mail synchronization, each of the `to`, `cc`, and `bcc` arrays can contain only one recipient
- the daily sender limit and the daily and monthly Bitrix24 e-mail limits apply

If a limit is exceeded, the e-mail is not sent and the method returns an error describing the limit.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.message.send`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"from":"user@example.com","to":["client@example.com"],"subject":"Business Proposal","body":"Hello. I am sending the materials."}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.message.send
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"from":"user@example.com","to":["client@example.com"],"subject":"Business Proposal","body":"Hello. I am sending the materials.","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.message.send
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SendMessageResult = {
      success: boolean
      to: string[]
    }

    try {
      const response = await $b24.actions.v3.call.make<SendMessageResult>({
        method: 'mail.message.send',
        params: {
          from: 'user@example.com',
          to: ['client@example.com'],
          subject: 'Business Proposal',
          body: 'Hello. I am sending the materials.',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Sent:', result.success, 'Recipients:', result.to)
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
      async function sendMessage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'mail.message.send',
            params: {
              from: 'user@example.com',
              to: ['client@example.com'],
              subject: 'Business Proposal',
              body: 'Hello. I am sending the materials.',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Sent:', result.success, 'Recipients:', result.to)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', sendMessage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    to = [
        "client@example.com",
    ]

    try:
        bitrix_response = client.mail.message.send(
            from_='user@example.com',
            to=to,
            subject='Quote',
            body='Hello. Sending the materials.',
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

- PHP

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mail.message.send',
                [
                    'from' => 'user@example.com',
                    'to' => ['client@example.com'],
                    'subject' => 'Business Proposal',
                    'body' => 'Hello. I am sending the materials.'
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'mail.message.send',
        {
            from: 'user@example.com',
            to: ['client@example.com'],
            subject: 'Business Proposal',
            body: 'Hello. I am sending the materials.'
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'mail.message.send',
        [
            'from' => 'user@example.com',
            'to' => ['client@example.com'],
            'subject' => 'Business Proposal',
            'body' => 'Hello. I am sending the materials.'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "mail.message.send", b24.Params{
    	"from":    "user@example.com",
    	"to":      []string{"client@example.com"},
    	"subject": "Business Proposal",
    	"body":    "Hello. I am sending the materials.",
    })
    if err != nil {
    	return fmt.Errorf("mail.message.send: %w", err)
    }

    var item struct {
    	Success bool     `json:"success"`
    	To      []string `json:"to"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Success, item.To)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "success": true,
        "to": [
            "client@example.com"
        ]
    },
    "time": {
        "start": 1779819678,
        "finish": 1779819678.84803,
        "duration": 0.8480300903320312,
        "processing": 0,
        "date_start": "2026-05-26T21:21:18+01:00",
        "date_finish": "2026-05-26T21:21:18+01:00",
        "operating_reset_at": 1779820278,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the sending result ||
|| **success**
[`boolean`](../../data-types.md) | Sending result. Always `true` in a successful response: if the e-mail is not sent, the method returns an error ||
|| **to**
[`array`](../../data-types.md) | Addresses from `to` to which the e-mail was sent. Recipient names in this array are replaced with the e-mail addresses found. CC and BCC recipients are not included ||
|| **time**
[`time`](../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object",
        "validation": [
            {
                "message": "Parameter \"from\" is required.",
                "field": "MISSING_FROM"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors
Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

In a response with this code, the `field` field contains the error reason code rather than the parameter name. The method checks parameters one by one and returns the first error found.

#|
|| **field value** | **Error description** | **How to fix** ||
|| `MISSING_FROM` | Parameter "from" is required. | Pass `from` as a non-empty string ||
|| `MISSING_SUBJECT` | Parameter "subject" is required. | Pass `subject` as a non-empty string ||
|| `MISSING_BODY` | Parameter "body" is required. | Pass `body` as a non-empty string ||
|| `MISSING_TO` | Parameter "to" is required and must be a non-empty array. | Pass `to` as a non-empty array. A string instead of an array also causes this error ||
|| `RESOLVE_RECIPIENTS_ERROR` | Recipient "zzqxnotexist987" not found in address book or among employees. | A recipient from `to`, `cc`, or `bcc` was not found by name. Pass an e-mail address. The method treats a value without `@` and a malformed e-mail address as a name ||
|| `RESOLVE_RECIPIENTS_ERROR` | Ambiguous recipient "Klaus". Found multiple matches: Klaus Weber &lt;klaus@example.com&gt;, Klaus Schmidt &lt;schmidt@example.com&gt;. Please specify the exact email address. | Several recipients were found by name. Pass an e-mail address from the list in the error text ||
|| `NO_RECIPIENTS` | No valid recipients provided. | `to` contains only empty strings or spaces. Pass at least one address ||
|| `MESSAGE_SEND_FAILED` | Sender identifiers must be positive integers. | `mailboxId` or `senderId` is less than 1. Pass values from the [mail.mailbox.senders](../mailbox/mail-mailbox-senders.md) response ||
|| `MESSAGE_SEND_FAILED` | Sender email is not available for this user. | The `from` address is not among the senders available to the user. Check the address using the [mail.mailbox.senders](../mailbox/mail-mailbox-senders.md) method ||
|| `MESSAGE_SEND_FAILED` | Sender is not available for this user. | The passed `mailboxId` or `senderId` does not match the `from` address. Take related values from the same record of the [mail.mailbox.senders](../mailbox/mail-mailbox-senders.md) response ||
|| `MESSAGE_SEND_FAILED` | Total number of recipients (to + cc + bcc) is 12, must not exceed 10. | The number of recipients exceeds the plan limit. Split the sending into several e-mails ||
|| `MESSAGE_SEND_FAILED` | Tariff restriction: each of to, cc, bcc must contain at most 1 recipient(s). | On the Bitrix24 plan, each array can contain only one recipient ||
|| `MESSAGE_SEND_FAILED` | Daily sender email limit reached. | The daily sender limit is exhausted. Retry the next day or use a different address ||
|| `MESSAGE_SEND_FAILED` | Daily portal mail limit reached. | The daily Bitrix24 e-mail limit is exhausted. Retry the next day ||
|| `MESSAGE_SEND_FAILED` | Monthly portal mail limit reached. | The monthly Bitrix24 e-mail limit is exhausted ||
|| `MESSAGE_SEND_FAILED` | Mailbox authentication failed. | The sender's mailbox failed authentication on the mail server. Check the mailbox connection settings ||
|| `MESSAGE_SEND_FAILED` | Failed to send email. | The e-mail could not be sent. Retry the request later ||
|#

If you pass a string instead of an array in `cc` or `bcc`, Bitrix24 returns HTTP status **500** and the `INTERNAL_SERVER_ERROR` error. Pass these parameters as arrays.

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-reply.md)
- [{#T}](./mail-message-forward.md)
- [{#T}](../mailbox/mail-mailbox-senders.md)
- [{#T}](../recipient/mail-recipient-listcontacts.md)
