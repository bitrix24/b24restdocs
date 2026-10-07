# How to Connect an SMS Provider to Bitrix24

> Scope: [`messageservice`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: an administrator must register the provider to complete the scenario
>
> - [messageservice.sender.add](../../api-reference/messageservice/messageservice-sender-add.md) — an administrator
> - [messageservice.message.status.update](../../api-reference/messageservice/messageservice-message-status-update.md) — the message sender or an administrator

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An SMS provider links Bitrix24 to an external messaging service. Once registered, users can send messages from CRM cards, Automation rules, and Workflows. The application handler passes each message to the external service, then updates its status in Bitrix24 after delivery is confirmed.

The delivery channel does not have to be SMS. A provider can pass a message to any service that identifies the recipient by phone number.

{% note info "" %}

For a detailed breakdown of the scenario, see the [SMS Integration](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268&LESSON_ID=26044) lesson.

{% endnote %}

The application registers the provider using [messageservice.sender.add](../../api-reference/messageservice/messageservice-sender-add.md) and passes the handler URL in `HANDLER`. After a message is sent, Bitrix24 calls this URL. The handler receives the number, text, and `message_id` and forwards the message to the external service. After delivery is confirmed, the application calls [messageservice.message.status.update](../../api-reference/messageservice/messageservice-message-status-update.md).

The scenario has four steps:

1. Register the provider using [messageservice.sender.add](../../api-reference/messageservice/messageservice-sender-add.md)
2. Receive the message in the `HANDLER` handler, send it to the external service, and retain `message_id`
3. Send a test message from a CRM card and verify that the handler was called
4. Update the delivery status using [messageservice.message.status.update](../../api-reference/messageservice/messageservice-message-status-update.md)

The order matters: Bitrix24 calls `HANDLER` only after the provider is registered, and the `MESSAGE_ID` needed to update the status appears in the handler request.

## Prepare the Data {#start}

Create an [application](../../settings/app-installation/index.md) or a [Local Application](../../settings/app-installation/local-apps/index.md) with the [`messageservice`](../../api-reference/scopes/permissions.md) scope. Retain the authorization data after installation and host the handler on an external server. The handler example requires PHP with the cURL extension and access to the external messaging service API.

Prepare the values you will replace with your own:

#|
|| **Value** | **Where to Get It** ||
|| `HANDLER_URL` | Public HTTPS URL of `handler.php`, such as `https://provider.example/api/handler.php?key=YOUR_SECRET` ||
|| `HANDLER_SECRET` | Generate a long random secret for the `key` URL parameter ||
|| `PROVIDER_API_URL` | Message sending endpoint of the external service ||
|| `PROVIDER_API_TOKEN` | Access token for the external service API ||
|| `MESSAGE_ID_LOG` | Path to a file outside the web server directory where PHP can write `message_id` ||
|#

Pass `HANDLER_URL` in `HANDLER`. Store the secret from the URL on the application server in the `HANDLER_SECRET` environment variable. Store the other values in PHP environment variables as well.

The [messageservice.sender.add](../../api-reference/messageservice/messageservice-sender-add.md) and [messageservice.message.status.update](../../api-reference/messageservice/messageservice-message-status-update.md) methods only work within the context of an installed application. Call them from the application interface via the JS SDK or from the application server using an OAuth token. An inbound webhook is not suitable for this scenario: the methods will return error `Application context required`.

If an application with an interface performs configuration in the installation wizard, complete the installation according to the rules on the [Completing Application Installation](../../settings/app-installation/installation-finish.md) page.

{% note warning "" %}

The handler URL in `HANDLER` must be accessible from the external network. Do not use `localhost`, local network addresses, or self-signed SSL certificates. Do not publish secrets in a repository or write the full handler URL to logs.

{% endnote %}

## 1. Register the Provider {#register}

Register the provider using [messageservice.sender.add](../../api-reference/messageservice/messageservice-sender-add.md). Pass four main parameters:

- `CODE` — the symbolic code of the provider. This code distinguishes the current application's provider from other providers in Bitrix24. Allowed characters: `a-z`, `A-Z`, `0-9`, `.`, `-`, `_`
- `TYPE` — the provider type. For an SMS provider, pass the value `SMS`
- `HANDLER` — the application handler URL
- `NAME` — the provider name that users will see in the Bitrix24 interface

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'messageservice.sender.add',
        {
            CODE: 'provider1',
            TYPE: 'SMS',
            HANDLER: 'https://provider.example/api/handler.php?key=YOUR_SECRET',
            NAME: 'SMS provider'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- Python

    ```python
    import requests

    rest_url = "https://your-domain.bitrix24.com/rest/messageservice.sender.add"
    payload = {
        "CODE": "provider1",
        "TYPE": "SMS",
        "HANDLER": "https://provider.example/api/handler.php?key=YOUR_SECRET",
        "NAME": "SMS provider",
        "auth": "put_access_token_here",
    }

    response = requests.post(rest_url, json=payload, timeout=30)
    response.raise_for_status()

    result = response.json()
    if "error" in result:
        raise RuntimeError(f"{result['error']}: {result.get('error_description', '')}")

    print(result["result"])
    ```


- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'messageservice.sender.add',
        [
            'CODE' => 'provider1',
            'TYPE' => 'SMS',
            'HANDLER' => 'https://provider.example/api/handler.php?key=YOUR_SECRET',
            'NAME' => 'SMS provider',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```
{% endlist %}

If the provider is successfully registered, the method returns `true`.

```json
{
    "result": true,
    "time": {
        "start": 1742895600,
        "finish": 1742895600.845505,
        "duration": 0.8455052375793457,
        "processing": 0,
        "date_start": "2025-03-25T10:00:00+03:00",
        "date_finish": "2025-03-25T10:00:00+03:00",
        "operating_reset_at": 1742896200,
        "operating": 0
    }
}
```

If you need to change the handler URL, name, or description of the provider, call [messageservice.sender.update](../../api-reference/messageservice/messageservice-sender-update.md). You can retrieve the code of an already registered provider using the [messageservice.sender.list](../../api-reference/messageservice/messageservice-sender-list.md) method.

## 2. Receive the Message in the Handler {#handler}

When a user or Automation sends a message, Bitrix24 calls the URL in `HANDLER`. Host a handler at that URL to receive the message data and information about where it was sent from.

The main fields required by the application to send a message to an external service are:

- `message_to` — the recipient's phone number
- `message_body` — the message text
- `message_id` — the external message identifier. Retain this if you intend to update the delivery status
- `module_id` — the tool from which the message was sent: `crm` for a CRM card or `bizproc` for a Workflow or CRM Automation rule
- `bindings` — CRM object links. This field is provided if `module_id=crm`
- `workflow_id`, `document_id`, `document_type` — Workflow data. These fields are provided if `module_id=bizproc`

If the message is sent from a CRM contact card, the data after parsing the POST request may look like this:

```json
{
    "module_id": "crm",
    "bindings": [
        {
            "OWNER_TYPE_ID": 3,
            "OWNER_ID": 123
        }
    ],
    "properties": {
        "phone_number": "+19990000000",
        "message_text": "Your confirmation code: 1234"
    },
    "type": "SMS",
    "code": "provider1",
    "message_id": "65575980fa531ac284c2ee68f81ebebd",
    "message_to": "+19990000000",
    "message_body": "Your confirmation code: 1234",
    "ts": 1742895600
}
```

See the full list of handler fields in the [messageservice.sender.add](../../api-reference/messageservice/messageservice-sender-add.md#handler) method description.

Bitrix24 sends a POST request to the handler. In PHP, the values are available in `$_POST`. Place the following code in `handler.php` at the URL specified in `HANDLER`. The example assumes that the external service accepts JSON with `to`, `text`, and `client_message_id` fields and a token in the `Authorization` header. Replace the field names, URL, and authorization method according to the chosen service's documentation.

```php
<?php
$secret = getenv('HANDLER_SECRET');
$providerUrl = getenv('PROVIDER_API_URL');
$providerToken = getenv('PROVIDER_API_TOKEN');
$messageIdLog = getenv('MESSAGE_ID_LOG');

if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
    http_response_code(405);
    exit;
}

if (!$secret || !hash_equals($secret, (string)($_GET['key'] ?? ''))) {
    http_response_code(403);
    exit;
}

$messageTo = trim((string)($_POST['message_to'] ?? ''));
$messageBody = (string)($_POST['message_body'] ?? '');
$messageId = (string)($_POST['message_id'] ?? '');
$senderCode = (string)($_POST['code'] ?? '');

if ($messageTo === '' || $messageBody === '' || $messageId === ''
    || strpbrk($messageId, "\r\n") !== false || $senderCode !== 'provider1') {
    http_response_code(400);
    exit;
}

if (!$providerUrl || !$providerToken || !$messageIdLog) {
    http_response_code(500);
    exit;
}

$payload = json_encode([
    'to' => $messageTo,
    'text' => $messageBody,
    'client_message_id' => $messageId,
], JSON_THROW_ON_ERROR);

$request = curl_init($providerUrl);
curl_setopt_array($request, [
    CURLOPT_POST => true,
    CURLOPT_POSTFIELDS => $payload,
    CURLOPT_HTTPHEADER => [
        'Content-Type: application/json',
        'Authorization: Bearer ' . $providerToken,
    ],
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_TIMEOUT => 15,
]);

$providerResponse = curl_exec($request);
$providerStatus = curl_getinfo($request, CURLINFO_RESPONSE_CODE);
curl_close($request);

if ($providerResponse === false || $providerStatus < 200 || $providerStatus >= 300) {
    error_log('Provider request failed for message_id=' . $messageId);
    http_response_code(502);
    exit;
}

if (file_put_contents($messageIdLog, $messageId . PHP_EOL, FILE_APPEND | LOCK_EX) === false) {
    http_response_code(500);
    exit;
}

http_response_code(200);
```

`MESSAGE_ID_LOG` must point to a writable file outside the web server directory. The handler records only `message_id`, without the phone number, text, or token. In a production application, retain the ID and the external service response in your own storage: you will need them when the service reports delivery. A `200` response means the external service accepted the request; it does not itself confirm delivery to the recipient.

## 3. Send a Test Message

After registering the provider and deploying the handler, send a test message from the Bitrix24 interface.

1. Open a CRM card containing a customer's phone number
2. Click **SMS/WhatsApp**
3. Check that your application's provider appears in the list
4. Enter a message and send it
5. Check that the handler received the request and retained `message_id`

The provider should also be available in Automation. Open the CRM Automation rule settings, add a **Send SMS** Automation rule, and check the provider list. The application follows the same flow: Bitrix24 sends the message data to the handler specified in `HANDLER`.

## 4. Update the Delivery Status {#status}

When the external service confirms delivery, the application can show the status in Bitrix24. Take the `message_id` retained by the handler and pass these parameters to [messageservice.message.status.update](../../api-reference/messageservice/messageservice-message-status-update.md):

- `CODE` — provider code
- `MESSAGE_ID` — the `message_id` from `MESSAGE_ID_LOG` or the application storage. The value `65575980fa531ac284c2ee68f81ebebd` below is an example; replace it with your message's ID
- `STATUS` — the new delivery status, for example `delivered`

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'messageservice.message.status.update',
        {
            CODE: 'provider1',
            MESSAGE_ID: '65575980fa531ac284c2ee68f81ebebd',
            STATUS: 'delivered'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- Python

    ```python
    import requests

    rest_url = "https://your-domain.bitrix24.com/rest/messageservice.message.status.update"
    payload = {
        "CODE": "provider1",
        "MESSAGE_ID": "65575980fa531ac284c2ee68f81ebebd",
        "STATUS": "delivered",
        "auth": "put_access_token_here",
    }

    response = requests.post(rest_url, json=payload, timeout=30)
    response.raise_for_status()

    result = response.json()
    if "error" in result:
        raise RuntimeError(f"{result['error']}: {result.get('error_description', '')}")

    print(result["result"])
    ```


- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'messageservice.message.status.update',
        [
            'CODE' => 'provider1',
            'MESSAGE_ID' => '65575980fa531ac284c2ee68f81ebebd',
            'STATUS' => 'delivered',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```
{% endlist %}

If the status is successfully updated, the method returns `true`.

```json
{
    "result": true,
    "time": {
        "start": 1742895600,
        "finish": 1742895600.425581,
        "duration": 0.4255819320678711,
        "processing": 0,
        "date_start": "2025-03-25T10:00:00+03:00",
        "date_finish": "2025-03-25T10:00:00+03:00",
        "operating_reset_at": 1742896200,
        "operating": 0
    }
}
```

The method supports the following statuses:

- `queued` — the message is enqueued for sending
- `sent` — the message has been sent by the provider
- `delivered` — the message was delivered to the recipient
- `undelivered` — the message was not delivered to the recipient
- `failed` — a sending or message processing error occurred at the provider

## Verify the Result

1. Check that [messageservice.sender.add](../../api-reference/messageservice/messageservice-sender-add.md) returned `result: true` and `provider1` appears in the sender list in the CRM card
2. Send a message from the CRM card. The handler should respond with `200`, and its `message_id` should appear in `MESSAGE_ID_LOG`. Confirm that the external service accepted the message with the same ID
3. After delivery is confirmed, call [messageservice.message.status.update](../../api-reference/messageservice/messageservice-message-status-update.md) with the retained `MESSAGE_ID` and `delivered` status. A `result: true` response confirms that Bitrix24 accepted the update

The delivery status should appear on the message in the CRM card.

## Errors and Diagnostics

If the message was not sent or the status was not updated, check where the scenario stopped:

- the provider is missing from the list — check the `messageservice.sender.add` result, `messageservice` scope, administrator permissions, and `HANDLER` URL; then register the provider again
- `messageservice.sender.add` returns `ERROR_SENDER_ALREADY_INSTALLED` — a provider with this `CODE` is already registered; check it with [messageservice.sender.list](../../api-reference/messageservice/messageservice-sender-list.md) and change the URL with [messageservice.sender.update](../../api-reference/messageservice/messageservice-sender-update.md)
- the handler responds with `403` — check that the secret in the `HANDLER` URL matches `HANDLER_SECRET`; then send the message again
- the handler responds with `400` — check that the incoming POST request contains `message_to`, `message_body`, `message_id`, and the `provider1` provider code; then send the message again
- the handler responds with `502` — check the external service URL, token, and request format; then send a new test message
- the handler responds with `500` — check the environment variables and write access to `MESSAGE_ID_LOG`; then send a new message
- `messageservice.message.status.update` returns `ERROR_MESSAGE_NOT_FOUND` — pass the `message_id` from the current `HANDLER` request and check the provider `CODE`; then retry the status update
- the method returns `ERROR_MESSAGE_STATUS_INCORRECT` — pass one of the supported `STATUS` values and retry
- the method returns `Application context required` — call it with OAuth authorization for the installed application instead of an inbound webhook

## Things to Consider

- Bitrix24 sends the message to the handler asynchronously. Sending from the interface and a `200` handler response do not prove delivery to the recipient: pass `delivered` only after confirmation from the external service
- A repeated request may cause the external service to receive the message twice. If it supports an idempotency key, use `message_id`
- The secret in the `HANDLER` URL grants access to the handler. Do not log the URL with the `key` parameter, and replace the secret if it is exposed
- If the application serves multiple Bitrix24 accounts, associate each request with the correct application installation

## Continue Learning

- [messageservice.sender.list](../../api-reference/messageservice/messageservice-sender-list.md) — retrieve registered provider codes
- [messageservice.sender.update](../../api-reference/messageservice/messageservice-sender-update.md) — change the handler URL
- [Handler Security](../../api-reference/events/safe-event-handlers.md) — verify the application token in incoming requests
