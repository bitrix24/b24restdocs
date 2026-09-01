# How to Send an E-mail from a Connected Mailbox

> Scope: [`mail`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire tutorial, the strictest of the listed permissions is required — access to a mailbox
>
> - [mail.mailbox.senders](../../api-reference/mail/mailbox/mail-mailbox-senders.md) — any user
> - [mail.recipient.listcontacts](../../api-reference/mail/recipient/mail-recipient-listcontacts.md) — any user
> - [mail.message.send](../../api-reference/mail/message/mail-message-send.md) — a user with access to the mailbox

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An e-mail can be sent from an address available to the current user in Bitrix24 Mail. First retrieve the available senders, then find the recipient address in the address book and send the e-mail.

The tutorial consists of three steps.

1. Retrieve senders using [mail.mailbox.senders](../../api-reference/mail/mailbox/mail-mailbox-senders.md)
2. Find a recipient using [mail.recipient.listcontacts](../../api-reference/mail/recipient/mail-recipient-listcontacts.md)
3. Send an e-mail using [mail.message.send](../../api-reference/mail/message/mail-message-send.md)

At step 3, the [mail.message.send](../../api-reference/mail/message/mail-message-send.md) method will return `success: true`, and the e-mail will be sent to the recipient address.

## What You Need Before You Start

Before running the tutorial, check that:

- an incoming webhook is created with the `mail` scope
- the webhook user has access to at least one connected mailbox
- the recipient exists in the address book or you know their e-mail address
- the webhook path is stored in an environment variable and contains the `/rest/api/` segment

Mail methods belong to REST 3.0. Method call specifics and the JSON request format are described in the [REST 3.0 overview](../../api-reference/rest-v3.md). For server-side JS examples, use `$b24.actions.v3`; for Python, specify `prefer_version=3`. The PHP SDK does not support calls through `/rest/api/`, so the PHP example sends a direct HTTP request.

The examples below use the sender address `user@example.com` and the recipient address `client@example.com`. In your Bitrix24, these values will be different: take the sender from the `mail.mailbox.senders` response and the recipient from the `mail.recipient.listcontacts` response or from your own data.

## 1. Retrieve Available Senders

The [mail.mailbox.senders](../../api-reference/mail/mailbox/mail-mailbox-senders.md) method returns the addresses that the current user can use to send e-mails.

Call the method with the parameter:

- `pagination` — pagination settings. In the example, we request the first page and limit the response to 20 senders

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    import { B24Hook, Text } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/api/USER_ID/TOKEN/'

    async function callMethod(method, params) {
        const response = await $b24.actions.v3.call.make({
            method,
            params,
            requestId: Text.getUuidRfc4122()
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    const sendersResult = await callMethod('mail.mailbox.senders', {
        pagination: {
            page: 1,
            limit: 20,
            offset: 0
        }
    })

    const sender = sendersResult.items[0]
    if (!sender) {
        throw new Error('No available senders')
    }
    ```

- PHP

    ```php
    <?php

    $webhook = getenv('B24_HOOK');
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/api/USER_ID/TOKEN/'

    function callMethod(string $webhook, string $method, array $params): array
    {
        $ch = curl_init($webhook . $method);
        curl_setopt_array($ch, [
            CURLOPT_POST => true,
            CURLOPT_HTTPHEADER => ['Content-Type: application/json', 'Accept: application/json'],
            CURLOPT_POSTFIELDS => json_encode($params, JSON_UNESCAPED_UNICODE),
            CURLOPT_RETURNTRANSFER => true,
        ]);

        $response = curl_exec($ch);
        if ($response === false)
        {
            throw new RuntimeException(curl_error($ch));
        }

        $data = json_decode($response, true);
        if (isset($data['error']))
        {
            throw new RuntimeException($data['error']['message']);
        }

        return $data['result'];
    }

    $sendersResult = callMethod($webhook, 'mail.mailbox.senders', [
        'pagination' => [
            'page' => 1,
            'limit' => 20,
            'offset' => 0,
        ],
    ]);

    $sender = $sendersResult['items'][0] ?? null;
    if (!$sender)
    {
        throw new RuntimeException('No available senders');
    }
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token=os.environ["B24_HOOK_TOKEN"],
        ),
        prefer_version=3,
    )
    # B24_HOOK_TOKEN = 'user_id/webhook_key'

    senders_result = client.mail.mailbox.senders(
        pagination={
            "page": 1,
            "limit": 20,
            "offset": 0,
        },
    ).response.result

    if not senders_result["items"]:
        raise RuntimeError("No available senders")

    sender = senders_result["items"][0]
    ```

{% endlist %}

As a result, you receive a list of available senders. For the next step, save `sender.email` — this address will be passed in `from`.

```json
{
    "result": {
        "items": [
            {
                "email": "user@example.com",
                "name": "Klaus Weber",
                "sender": "Klaus Weber <user@example.com>"
            }
        ]
    }
}
```

## 2. Find the Recipient

The [mail.recipient.listcontacts](../../api-reference/mail/recipient/mail-recipient-listcontacts.md) method searches contacts in the current user's address book.

Use the method with the parameters:

- `query` — client name or e-mail. In the example, we search for the address `client@example.com`
- `pagination` — pagination settings. In the example, we request the first page and limit the response to 20 contacts

{% list tabs %}

- JS

    ```js
    const recipientsResult = await callMethod('mail.recipient.listcontacts', {
        query: 'client@example.com',
        pagination: {
            page: 1,
            limit: 20,
            offset: 0
        }
    })

    const recipientEmail = recipientsResult.items[0]?.email ?? 'client@example.com'
    ```

- PHP

    ```php
    $recipientsResult = callMethod($webhook, 'mail.recipient.listcontacts', [
        'query' => 'client@example.com',
        'pagination' => [
            'page' => 1,
            'limit' => 20,
            'offset' => 0,
        ],
    ]);

    $recipientEmail = $recipientsResult['items'][0]['email'] ?? 'client@example.com';
    ```

- Python

    ```python
    recipients_result = client.mail.recipient.listcontacts(
        query="client@example.com",
        pagination={
            "page": 1,
            "limit": 20,
            "offset": 0,
        },
    ).response.result

    recipient_email = recipients_result["items"][0]["email"] if recipients_result["items"] else "client@example.com"
    ```

{% endlist %}

As a result, you receive a list of contacts. For the next step, save the recipient's e-mail address in the `recipientEmail` variable. If there is no address book entry, pass the known e-mail address directly in the `to` array.

```json
{
    "result": {
        "items": [
            {
                "id": 10,
                "email": "client@example.com",
                "name": "Client"
            }
        ]
    }
}
```

## 3. Send the E-mail

The [mail.message.send](../../api-reference/mail/message/mail-message-send.md) method sends a new e-mail.

Use the method with the parameters:

- `from` — sender address from the `email` field in step 1
- `to` — array of recipient addresses. In the example, we pass the e-mail from step 2, and if the address book returned nothing, the known address `client@example.com`
- `subject` — e-mail subject
- `body` — e-mail text

{% note warning "" %}

The following request sends a real e-mail. Debug the tutorial using a test address.

{% endnote %}

{% list tabs %}

- JS

    ```js
    const sendResult = await callMethod('mail.message.send', {
        from: sender.email,
        to: [recipientEmail],
        subject: 'Commercial proposal',
        body: 'Hello. I am sending the materials.'
    })

    console.log(sendResult)
    ```

- PHP

    ```php
    $sendResult = callMethod($webhook, 'mail.message.send', [
        'from' => $sender['email'],
        'to' => [$recipientEmail],
        'subject' => 'Commercial proposal',
        'body' => 'Hello. I am sending the materials.',
    ]);

    print_r($sendResult);
    ```

- Python

    ```python
    send_result = client.mail.message.send(
        from_=sender["email"],
        to=[recipient_email],
        subject="Commercial proposal",
        body="Hello. I am sending the materials.",
    ).response.result

    print(send_result)
    ```

{% endlist %}

A successful response contains the sending flag and the list of addresses to which the e-mail was sent.

```json
{
    "result": {
        "success": true,
        "to": [
            "client@example.com"
        ]
    }
}
```

## Verify the Result

Check the recipient's mailbox: the e-mail must arrive with the subject from `subject` and the text from `body`.

Through REST, successful sending is confirmed by the `mail.message.send` response: `success` is `true`, and the `to` array contains the recipient address.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and Action** ||
|| `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION` | The webhook or application does not have the `mail` scope. Add the scope and repeat the request ||
|| `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` | The user does not have access to the mailbox or sender. Check which user the webhook was created for ||
|| `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION` | A required field, `from`, `to`, `subject`, or `body`, is not filled in the request. Check the JSON body ||
|| `MESSAGE_SEND_FAILED` | The address from `from` is not available to the user, or the e-mail could not be sent. Retrieve the sender again using `mail.mailbox.senders` ||
|| `NO_RECIPIENTS` | There are no valid recipients in `to`. Check that the array contains e-mail addresses ||
|| `RESOLVE_RECIPIENTS_ERROR` | Bitrix24 could not recognize the addresses in `to`, `cc`, or `bcc`. Pass e-mail addresses as strings ||
|#

Repeat the tutorial from the step that returned the error. Steps 1 and 2 do not create anything, so they can be executed again.

## Key Considerations

Consider the tutorial limitations:

- `mail.message.send` sends an e-mail from Mail but does not create a CRM activity
- `mail.message.send` does not return the identifier of the created e-mail, so the result cannot be passed directly to [mail.message.createcrmactivity](../../api-reference/mail/message/mail-message-createcrmactivity.md)
- if you need to send a copy or blind copy of the e-mail, pass the optional `cc` and `bcc` parameters of the `mail.message.send` method as arrays of e-mail addresses
- running the example again will send another e-mail

## Code Example

The code combines all three steps: retrieves the sender, finds the recipient, and sends the e-mail. Replace `client@example.com`, `subject`, and `body` with your own values.

{% list tabs %}

- JS

    ```js
    import { B24Hook, Text } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    async function callMethod(method, params) {
        const response = await $b24.actions.v3.call.make({
            method,
            params,
            requestId: Text.getUuidRfc4122()
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    const senders = await callMethod('mail.mailbox.senders', {
        pagination: { page: 1, limit: 20, offset: 0 }
    })
    const sender = senders.items[0]
    if (!sender) {
        throw new Error('No available senders')
    }

    const recipients = await callMethod('mail.recipient.listcontacts', {
        query: 'client@example.com',
        pagination: { page: 1, limit: 20, offset: 0 }
    })
    const recipientEmail = recipients.items[0]?.email ?? 'client@example.com'

    const result = await callMethod('mail.message.send', {
        from: sender.email,
        to: [recipientEmail],
        subject: 'Commercial proposal',
        body: 'Hello. I am sending the materials.'
    })

    console.log(result)
    ```

- PHP

    ```php
    <?php

    $webhook = getenv('B24_HOOK');

    function callMethod(string $webhook, string $method, array $params): array
    {
        $ch = curl_init($webhook . $method);
        curl_setopt_array($ch, [
            CURLOPT_POST => true,
            CURLOPT_HTTPHEADER => ['Content-Type: application/json', 'Accept: application/json'],
            CURLOPT_POSTFIELDS => json_encode($params, JSON_UNESCAPED_UNICODE),
            CURLOPT_RETURNTRANSFER => true,
        ]);

        $response = curl_exec($ch);
        if ($response === false)
        {
            throw new RuntimeException(curl_error($ch));
        }

        $data = json_decode($response, true);
        if (isset($data['error']))
        {
            throw new RuntimeException($data['error']['message']);
        }

        return $data['result'];
    }

    $senders = callMethod($webhook, 'mail.mailbox.senders', [
        'pagination' => ['page' => 1, 'limit' => 20, 'offset' => 0],
    ]);
    $sender = $senders['items'][0] ?? null;
    if (!$sender)
    {
        throw new RuntimeException('No available senders');
    }

    $recipients = callMethod($webhook, 'mail.recipient.listcontacts', [
        'query' => 'client@example.com',
        'pagination' => ['page' => 1, 'limit' => 20, 'offset' => 0],
    ]);
    $recipientEmail = $recipients['items'][0]['email'] ?? 'client@example.com';

    $result = callMethod($webhook, 'mail.message.send', [
        'from' => $sender['email'],
        'to' => [$recipientEmail],
        'subject' => 'Commercial proposal',
        'body' => 'Hello. I am sending the materials.',
    ]);

    print_r($result);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token=os.environ["B24_HOOK_TOKEN"],
        ),
        prefer_version=3,
    )

    senders = client.mail.mailbox.senders(
        pagination={"page": 1, "limit": 20, "offset": 0},
    ).response.result
    if not senders["items"]:
        raise RuntimeError("No available senders")

    sender = senders["items"][0]

    recipients = client.mail.recipient.listcontacts(
        query="client@example.com",
        pagination={"page": 1, "limit": 20, "offset": 0},
    ).response.result
    recipient_email = recipients["items"][0]["email"] if recipients["items"] else "client@example.com"

    result = client.mail.message.send(
        from_=sender["email"],
        to=[recipient_email],
        subject="Commercial proposal",
        body="Hello. I am sending the materials.",
    ).response.result

    print(result)
    ```

{% endlist %}

## Continue Learning

- [Retrieve Senders mail.mailbox.senders](../../api-reference/mail/mailbox/mail-mailbox-senders.md)
- [Retrieve a List of Contacts mail.recipient.listcontacts](../../api-reference/mail/recipient/mail-recipient-listcontacts.md)
- [Send an E-mail mail.message.send](../../api-reference/mail/message/mail-message-send.md)
- [Mail in REST 3.0: Section Overview](../../api-reference/mail/index.md)
- [REST 3.0 Overview](../../api-reference/rest-v3.md)
