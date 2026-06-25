# How to Connect an SMS Provider to Bitrix24

> Scope: [`messageservice`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: an administrator manages providers. The message sender or an administrator updates the delivery status

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An SMS provider links Bitrix24 to an external messaging service. Once the provider is registered, users can send messages from CRM cards, Automation rules, and Workflows.

The delivery channel does not have to be SMS. A provider can pass a message to any service that identifies the recipient by phone number.

{% note info "" %}

For a detailed breakdown of the scenario, see the [SMS Integration](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268&LESSON_ID=26044) lesson.

{% endnote %}

## How the Integration Works {#workflow}

Three parties are involved in the scenario:

- Bitrix24 — displays the provider in the interface and passes message data to the application
- The provider application — receives the request from Bitrix24 and links it to the external sending service
- The external service — sends the message to the recipient

Before sending, the application code registers the provider using the [messageservice.sender.add](../../api-reference/messageservice/messageservice-sender-add.md) method. In the request, it passes `HANDLER` — the URL of the handler on the application server. This URL is prepared in advance by the application developer: the handler must be accessible from the internet and must accept requests from Bitrix24.

When a user or an Automation rule sends a message, Bitrix24 calls `HANDLER` and passes the recipient's number, the message text, and service data to the application. The application sends the message to the external service and can then pass the delivery status to Bitrix24.

In this scenario, we will use the following methods:

1. [messageservice.sender.add](../../api-reference/messageservice/messageservice-sender-add.md) — to register the SMS provider
2. [messageservice.message.status.update](../../api-reference/messageservice/messageservice-message-status-update.md) — to update the message delivery status

Next, we will break down this scenario step by step: prepare the application, register the provider, test sending from Bitrix24, and configure status handling.

## 1. Preparing the Application {#start}

To test the scenario, create an [application](../../settings/app-installation/index.md) or a [Local application](../../settings/app-installation/local-apps/index.md). The application requires the [`messageservice`](../../api-reference/scopes/permissions.md) scope, saved authorization data after installation, and a handler on an external server.

The [messageservice.sender.add](../../api-reference/messageservice/messageservice-sender-add.md) and [messageservice.message.status.update](../../api-reference/messageservice/messageservice-message-status-update.md) methods only work within the context of an installed application. Call them from the application interface via the JS SDK or from the application server using an OAuth token. An incoming webhook is not suitable for this scenario: the methods will return error `Application context required`.

If an application with an interface performs configuration in the installation wizard, complete the installation according to the rules on the [Completing Application Installation](../../settings/app-installation/installation-finish.md) page.

{% note info "" %}

The handler URL from the `HANDLER` parameter must be accessible from an external network. Do not use `localhost`, local network addresses, or self-signed SSL certificates.

{% endnote %}

## 2. Registering a Provider {#register}

A provider is registered using the [messageservice.sender.add](../../api-reference/messageservice/messageservice-sender-add.md) method. The request must pass four main parameters to the application:

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
            HANDLER: 'https://provider.example/api/handler',
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'messageservice.sender.add',
        [
            'CODE' => 'provider1',
            'TYPE' => 'SMS',
            'HANDLER' => 'https://provider.example/api/handler',
            'NAME' => 'SMS provider',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    ```python
    import requests

    rest_url = "https://your-domain.bitrix24.com/rest/messageservice.sender.add"
    payload = {
        "CODE": "provider1",
        "TYPE": "SMS",
        "HANDLER": "https://provider.example/api/handler",
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

## 3. Verifying Sending from Bitrix24

After registering the provider, send a test message from the Bitrix24 interface.

1. Open a CRM card containing a customer's phone number
2. Click **SMS/WhatsApp**
3. Verify that the provider from your application is available in the list
4. Enter the message text and send it
5. Verify that the handler received the request

The provider must also be available in Automation. Open the CRM Automation rule settings, add a **Send SMS** Automation rule, and check the provider list. For the application, the scenario is the same: Bitrix24 will send the message data to the handler via the `HANDLER` parameter.

## 4. Processing the Bitrix24 Request {#handler}

When a user or Automation sends a message, Bitrix24 calls the URL from the `HANDLER` parameter. The handler receives the message data and details about the scenario from which it was sent.

The main fields required by the application to send a message to an external service are:

- `message_to` — the recipient's phone number
- `message_body` — the message text
- `message_id` — the external message identifier. Retain this if you intend to update the delivery status
- `module_id` — the tool from which the message was sent: `crm` for a CRM card or `bizproc` for a Workflow or CRM Automation rule
- `bindings` — CRM object links. This field is provided if `module_id=crm`
- `workflow_id`, `document_id`, `document_type` — Workflow data. These fields are provided if `module_id=bizproc`

If the message is sent from a CRM contact card, the incoming data may look like this:

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

## 5. Updating the Delivery Status {#status}

If an external service returns a delivery result, the application can display it in Bitrix24. To do this, save the `message_id` from the request to the handler and pass the following parameters to the [messageservice.message.status.update](../../api-reference/messageservice/messageservice-message-status-update.md) method:

- `CODE` — provider code
- `MESSAGE_ID` — the saved `message_id`
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