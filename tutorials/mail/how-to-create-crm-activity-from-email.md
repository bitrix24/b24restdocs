# How to Create a CRM Activity from an Incoming E-mail

> Scope: [`mail`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire tutorial, the strictest of the listed permissions is required — access to the mailbox where the e-mail is located and access to CRM
>
> - [mail.mailbox.list](../../api-reference/mail/mailbox/mail-mailbox-list.md) — any user
> - [mail.message.list](../../api-reference/mail/message/mail-message-list.md) — any user
> - [mail.message.createcrmactivity](../../api-reference/mail/message/mail-message-createcrmactivity.md) — a user with access to the mailbox where the e-mail is located and access to CRM
> - [mail.message.get](../../api-reference/mail/message/mail-message-get.md) — a user with access to the mailbox where the e-mail is located

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An incoming e-mail can be converted into a CRM activity. To do this, find the e-mail in an available mailbox and pass its identifier to the activity creation method.

The [mail.message.createcrmactivity](../../api-reference/mail/message/mail-message-createcrmactivity.md) method does not accept a lead, deal, contact, or company identifier. It creates a CRM activity from an e-mail, and the link to the CRM object is determined by the e-mail data and CRM settings.

The tutorial consists of four steps.

1. Retrieve mailboxes using [mail.mailbox.list](../../api-reference/mail/mailbox/mail-mailbox-list.md)
2. Find an incoming e-mail using [mail.message.list](../../api-reference/mail/message/mail-message-list.md)
3. Create a CRM activity using [mail.message.createcrmactivity](../../api-reference/mail/message/mail-message-createcrmactivity.md)
4. Check the e-mail link using [mail.message.get](../../api-reference/mail/message/mail-message-get.md)

As a result, the e-mail will have a link in the `bindings` field, and a CRM activity will be created from the e-mail.

## What You Need Before You Start

Before running the tutorial, check that:

- an incoming webhook is created with the `mail` scope
- the webhook user has access to the mailbox with the incoming e-mail
- CRM is enabled and configured, and the webhook user has CRM access
- the e-mail is available to the current user and has not been deleted
- the webhook path is stored in an environment variable and contains the `/rest/api/` segment

Mail methods belong to REST 3.0. Method call specifics and the JSON request format are described in the [REST 3.0 overview](../../api-reference/rest-v3.md). For server-side JS examples, use `$b24.actions.v3`; for Python, specify `prefer_version=3`. The PHP SDK does not support calls through `/rest/api/`, so the PHP example sends a direct HTTP request.

The examples below use an e-mail with the subject "Contract" and the period from August 1 through August 31, 2026. In your Bitrix24, the values will be different: choose the search string and period so that `mail.message.list` finds the required incoming e-mail.

## 1. Retrieve Mailboxes

The [mail.mailbox.list](../../api-reference/mail/mailbox/mail-mailbox-list.md) method returns the current user's mailboxes.

Call the method with the parameter:

- `pagination` — pagination settings. In the example, we request the first page and limit the response to 20 mailboxes

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

    const mailboxesResult = await callMethod('mail.mailbox.list', {
        pagination: {
            page: 1,
            limit: 20,
            offset: 0
        }
    })

    const mailbox = mailboxesResult.items[0]
    if (!mailbox) {
        throw new Error('No available mailboxes')
    }

    const mailboxId = mailbox.id
    ```

- PHP

    ```php
    <?php

    $webhook = getenv('B24_HOOK');
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/api/USER_ID/TOKEN/'

    function callMethod(string $webhook, string $method, array $params)
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

    $mailboxesResult = callMethod($webhook, 'mail.mailbox.list', [
        'pagination' => [
            'page' => 1,
            'limit' => 20,
            'offset' => 0,
        ],
    ]);

    $mailbox = $mailboxesResult['items'][0] ?? null;
    if (!$mailbox)
    {
        throw new RuntimeException('No available mailboxes');
    }

    $mailboxId = $mailbox['id'];
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

    mailboxes_result = client.mail.mailbox.list(
        pagination={
            "page": 1,
            "limit": 20,
            "offset": 0,
        },
    ).response.result

    if not mailboxes_result["items"]:
        raise RuntimeError("No available mailboxes")

    mailbox_id = mailboxes_result["items"][0]["id"]
    ```

{% endlist %}

As a result, you receive a list of mailboxes. For the next step, save the `id` of the mailbox where you need to find the incoming e-mail.

```json
{
    "result": {
        "items": [
            {
                "id": 1,
                "name": "Work mail",
                "email": "user@example.com",
                "senderName": "Klaus Weber"
            }
        ]
    }
}
```

## 2. Find the Incoming E-mail

The [mail.message.list](../../api-reference/mail/message/mail-message-list.md) method returns e-mails by conditions. In the example, we search for an e-mail in the selected mailbox by subject and period.

Use the method with the parameters:

- `mailboxId` — mailbox identifier from step 1
- `searchQuery` — e-mail search string. In the example, we search for e-mails by the word `contract`
- `dateFrom` and `dateTo` — boundaries of the period where the e-mail must be found
- `pagination` — pagination settings. In the example, we request the first page and limit the response to 20 e-mails

{% list tabs %}

- JS

    ```js
    const messagesResult = await callMethod('mail.message.list', {
        mailboxId,
        searchQuery: 'contract',
        dateFrom: '2026-08-01T00:00:00+02:00',
        dateTo: '2026-08-31T23:59:59+02:00',
        pagination: {
            page: 1,
            limit: 20,
            offset: 0
        }
    })

    const message = messagesResult.items[0]
    if (!message) {
        throw new Error('E-mail not found')
    }

    const messageId = message.id
    ```

- PHP

    ```php
    $messagesResult = callMethod($webhook, 'mail.message.list', [
        'mailboxId' => $mailboxId,
        'searchQuery' => 'contract',
        'dateFrom' => '2026-08-01T00:00:00+02:00',
        'dateTo' => '2026-08-31T23:59:59+02:00',
        'pagination' => [
            'page' => 1,
            'limit' => 20,
            'offset' => 0,
        ],
    ]);

    $message = $messagesResult['items'][0] ?? null;
    if (!$message)
    {
        throw new RuntimeException('E-mail not found');
    }

    $messageId = $message['id'];
    ```

- Python

    ```python
    messages_result = client.mail.message.list(
        mailbox_id=mailbox_id,
        search_query="contract",
        date_from="2026-08-01T00:00:00+02:00",
        date_to="2026-08-31T23:59:59+02:00",
        pagination={
            "page": 1,
            "limit": 20,
            "offset": 0,
        },
    ).response.result

    if not messages_result["items"]:
        raise RuntimeError("E-mail not found")

    message_id = messages_result["items"][0]["id"]
    ```

{% endlist %}

As a result, you receive a list of e-mails. For the next step, save the `id` of the required e-mail in the `messageId` variable.

```json
{
    "result": {
        "items": [
            {
                "id": 15,
                "mailboxId": 1,
                "mailboxEmail": "user@example.com",
                "subject": "Contract",
                "from": "client@example.com",
                "to": "user@example.com",
                "date": "2026-08-15T10:00:00+02:00",
                "bindings": []
            }
        ]
    }
}
```

## 3. Create a CRM Activity

The [mail.message.createcrmactivity](../../api-reference/mail/message/mail-message-createcrmactivity.md) method creates a CRM activity from an e-mail.

Use the method with the parameter:

- `messageId` — e-mail identifier saved from the [mail.message.list](../../api-reference/mail/message/mail-message-list.md) response in step 2

{% list tabs %}

- JS

    ```js
    const createResult = await callMethod('mail.message.createcrmactivity', {
        messageId
    })

    console.log(createResult)
    ```

- PHP

    ```php
    $createResult = callMethod($webhook, 'mail.message.createcrmactivity', [
        'messageId' => $messageId,
    ]);

    print_r($createResult);
    ```

- Python

    ```python
    create_result = client.mail.message.createcrmactivity(
        message_id=message_id,
    ).response.result

    print(create_result)
    ```

{% endlist %}

A successful response contains an object with `result: true`.

```json
{
    "result": {
        "result": true
    }
}
```

## 4. Check the E-mail Link

The [mail.message.get](../../api-reference/mail/message/mail-message-get.md) method returns an e-mail by identifier.

Use the method with the parameters:

- `id` — e-mail identifier `messageId` saved from the [mail.message.list](../../api-reference/mail/message/mail-message-list.md) response in step 2
- `select` — list of fields to retrieve. Request the `bindings` field to see the created link

{% list tabs %}

- JS

    ```js
    const messageResult = await callMethod('mail.message.get', {
        id: messageId,
        select: [
            'id',
            'subject',
            'from',
            'to',
            'bindings',
            'url'
        ]
    })

    console.log(messageResult.item.bindings)
    ```

- PHP

    ```php
    $messageResult = callMethod($webhook, 'mail.message.get', [
        'id' => $messageId,
        'select' => [
            'id',
            'subject',
            'from',
            'to',
            'bindings',
            'url',
        ],
    ]);

    print_r($messageResult['item']['bindings']);
    ```

- Python

    ```python
    message_result = client.mail.message.get(
        bitrix_id=message_id,
        select=[
            "id",
            "subject",
            "from",
            "to",
            "bindings",
            "url",
        ],
    ).response.result

    print(message_result["item"]["bindings"])
    ```

{% endlist %}

The CRM link is displayed in the `bindings` array. The response is shortened to the fields required for verification.

```json
{
    "result": {
        "item": {
            "id": 15,
            "subject": "Contract",
            "from": "client@example.com",
            "to": "user@example.com",
            "url": "/mail/message/15",
            "bindings": [
                {
                    "type": "crm",
                    "entityTypeId": 3,
                    "entityId": 125
                }
            ]
        }
    }
}
```

## Verify the Result

Open the e-mail in Bitrix24 Mail. The e-mail must have a CRM link.

Through REST, the tutorial is complete if the [mail.message.get](../../api-reference/mail/message/mail-message-get.md) method returns a non-empty `bindings` array and it contains an object with `type: "crm"`.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and Action** ||
|| `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION` | The webhook or application does not have the `mail` scope. Add the scope and repeat the request ||
|| `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` | The user does not have access to the mailbox or e-mail. Check the webhook user ||
|| `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION` | An empty or invalid value was passed in `messageId`, or the e-mail cannot be saved to CRM. Pass a positive integer and select an e-mail that can be linked to CRM ||
|| `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` | The e-mail was not found. Check `mailboxId`, the search filter, and the e-mail identifier ||
|| `MESSAGE_LIST_FAILED` | The e-mail search conditions did not pass validation. Check the `dateFrom` and `dateTo` format ||
|#

If the [mail.message.createcrmactivity](../../api-reference/mail/message/mail-message-createcrmactivity.md) method returns an object with `result: true`, but `bindings` is empty, check CRM settings and e-mail data:

- the webhook user has CRM access
- CRM tracker or e-mail processing in CRM is configured for the address from the e-mail
- the sender or recipient address of the e-mail matches an e-mail in a lead, contact, or company
- the e-mail has not been deleted and is available in the active mailbox connection

The method does not accept a target CRM object manually, so the link depends on CRM e-mail processing.

## Key Considerations

Consider the tutorial limitations:

- `mail.message.createcrmactivity` creates a CRM activity from an existing e-mail and does not send a new e-mail
- the `messageId` parameter of the `mail.message.createcrmactivity` method is taken from the [mail.message.list](../../api-reference/mail/message/mail-message-list.md) or [mail.message.get](../../api-reference/mail/message/mail-message-get.md) response
- the target CRM object cannot be passed as a parameter: `mail.message.createcrmactivity` has no fields for a lead, deal, contact, or company identifier
- calling `mail.message.createcrmactivity` again for the same e-mail may return an error or leave the existing link unchanged; check `bindings` before retrying
- the link can be deleted using [mail.message.removecrmactivity](../../api-reference/mail/message/mail-message-removecrmactivity.md)

## Code Example

The code combines all steps: retrieves a mailbox, searches for an e-mail, creates a CRM activity, and checks `bindings`. Replace the search string and period with your own values.

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

    const mailboxes = await callMethod('mail.mailbox.list', {
        pagination: { page: 1, limit: 20, offset: 0 }
    })
    const mailbox = mailboxes.items[0]
    if (!mailbox) {
        throw new Error('No available mailboxes')
    }
    const mailboxId = mailbox.id

    const messages = await callMethod('mail.message.list', {
        mailboxId,
        searchQuery: 'contract',
        dateFrom: '2026-08-01T00:00:00+02:00',
        dateTo: '2026-08-31T23:59:59+02:00',
        pagination: { page: 1, limit: 20, offset: 0 }
    })
    const sourceMessage = messages.items[0]
    if (!sourceMessage) {
        throw new Error('E-mail not found')
    }
    const messageId = sourceMessage.id

    await callMethod('mail.message.createcrmactivity', { messageId })

    const message = await callMethod('mail.message.get', {
        id: messageId,
        select: ['id', 'subject', 'from', 'to', 'bindings', 'url']
    })

    console.log(message.item.bindings)
    ```

- PHP

    ```php
    <?php

    $webhook = getenv('B24_HOOK');

    function callMethod(string $webhook, string $method, array $params)
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

    $mailboxes = callMethod($webhook, 'mail.mailbox.list', [
        'pagination' => ['page' => 1, 'limit' => 20, 'offset' => 0],
    ]);
    $mailbox = $mailboxes['items'][0] ?? null;
    if (!$mailbox)
    {
        throw new RuntimeException('No available mailboxes');
    }
    $mailboxId = $mailbox['id'];

    $messages = callMethod($webhook, 'mail.message.list', [
        'mailboxId' => $mailboxId,
        'searchQuery' => 'contract',
        'dateFrom' => '2026-08-01T00:00:00+02:00',
        'dateTo' => '2026-08-31T23:59:59+02:00',
        'pagination' => ['page' => 1, 'limit' => 20, 'offset' => 0],
    ]);
    $sourceMessage = $messages['items'][0] ?? null;
    if (!$sourceMessage)
    {
        throw new RuntimeException('E-mail not found');
    }
    $messageId = $sourceMessage['id'];

    callMethod($webhook, 'mail.message.createcrmactivity', [
        'messageId' => $messageId,
    ]);

    $message = callMethod($webhook, 'mail.message.get', [
        'id' => $messageId,
        'select' => ['id', 'subject', 'from', 'to', 'bindings', 'url'],
    ]);

    print_r($message['item']['bindings']);
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

    mailboxes = client.mail.mailbox.list(
        pagination={"page": 1, "limit": 20, "offset": 0},
    ).response.result
    if not mailboxes["items"]:
        raise RuntimeError("No available mailboxes")

    mailbox_id = mailboxes["items"][0]["id"]

    messages = client.mail.message.list(
        mailbox_id=mailbox_id,
        search_query="contract",
        date_from="2026-08-01T00:00:00+02:00",
        date_to="2026-08-31T23:59:59+02:00",
        pagination={"page": 1, "limit": 20, "offset": 0},
    ).response.result
    if not messages["items"]:
        raise RuntimeError("E-mail not found")

    message_id = messages["items"][0]["id"]

    create_result = client.mail.message.createcrmactivity(
        message_id=message_id,
    ).response.result

    message = client.mail.message.get(
        bitrix_id=message_id,
        select=["id", "subject", "from", "to", "bindings", "url"],
    ).response.result

    print(message["item"]["bindings"])
    ```

{% endlist %}

## Continue Learning

- [Retrieve a List of Mailboxes mail.mailbox.list](../../api-reference/mail/mailbox/mail-mailbox-list.md)
- [Retrieve a List of E-mails mail.message.list](../../api-reference/mail/message/mail-message-list.md)
- [Create a CRM Activity from an E-mail mail.message.createcrmactivity](../../api-reference/mail/message/mail-message-createcrmactivity.md)
- [Retrieve an E-mail mail.message.get](../../api-reference/mail/message/mail-message-get.md)
- [Delete the Link Between an E-mail and a CRM Activity mail.message.removecrmactivity](../../api-reference/mail/message/mail-message-removecrmactivity.md)
