# How to Create a Support Channel via Bitrix24 Network Open Channel

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Using a Bitrix24 Network Open Channel, you can organize support for application users. After executing the scenario, the user will receive a welcome message in their messenger from the Open Channel support.

The scenario links two Bitrix24 instances:

- Support Bitrix24 — the "Bitrix24 Network" channel is connected here and the Open Channel is configured. This is where the connector code is retrieved.
- User Bitrix24 — the application is installed here, which performs both REST calls and sends the welcome message to the user.

The scenario consists of two steps.

1. Connect the Open Channel using the [imopenlines.network.join](../../api-reference/imopenlines/openlines/imopenlines-network-join.md) method.
2. Send a welcome message to the user using the [imopenlines.network.message.add](../../api-reference/imopenlines/openlines/imopenlines-network-message-add.md) method.

Both methods use the same connector code. The network bot identifier from step 1 is not required to send the message.

> Scope: [`imopenlines`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: any user

## Prepare the Application and Open Channel

### Prepare the Application

In the user's Bitrix24, prepare a local application without a user interface.

1. Prepare a handler address accessible from the internet, for example `https://example.com/handler`
2. Create a [local application with an installation handler](../../settings/app-installation/local-apps/installation-callback.md) and enable the "Uses API only" option.
3. In the "Initial installation path" field, specify the handler address and grant the application the [`imopenlines`](../../api-reference/scopes/permissions.md) permission.
4. In the handler, retrieve the authorization data `auth` and [initialize the SDK](./index.md#initializing-the-sdk-using-event-data).

### Configure the Open Channel

Perform the following actions in the support Bitrix24.

1. Open the "Contact Center" section and connect the "Bitrix24 Network" communication channel.
2. Specify a name and a short description, and add an avatar — users will be able to recognize the support channel by these details.
3. Create a new support Open Channel or select an existing one.
4. Save the settings and copy the value of the "Code" field on the connector page.

### Prepare the Values

- `connectorCode` — the connector code from the "Code" field. This is a 32-character string.
- `userId` — the user identifier from `auth[user_id]` in the application installation data.
- `message` — non-empty welcome text.

The `CODE` parameter contains only the connector code. In the examples below, the SDK client substitutes the OAuth token. In a direct REST request without the SDK, the token is passed as a separate parameter `auth` or `access_token`, rather than within the method parameters.

### Initialize the SDK

Prepare the initialization functions following the [event data processing example](./index.md#initializing-the-sdk-using-event-data). In the installation handler, create an SDK client from the authorization data `auth`.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    const $b24 = makeClient(auth)
    ```

- PHP

    ```php
    $b24 = makeServiceBuilder($request);
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError

    client, token = make_client(auth)
    ```

{% endlist %}

The PHP client initialization example specifies the scope `imbot,im,task`. For the support scenario, replace it with `imopenlines`.

## 1. Connect an Open Channel

Pass the connector code into the `CODE` parameter of the [imopenlines.network.join](../../api-reference/imopenlines/openlines/imopenlines-network-join.md) method. In the examples, replace the demonstration value `connectorCode` with your own code. If the line is already connected, the method will return the identifier of the existing network bot.

{% list tabs %}

- JS

    ```js
    const connectorCode = 'a588e1a88baaf301b9d0b0b33b1eefc2'

    try {
        const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.network.join',
            params: { CODE: connectorCode },
            requestId: 'network-join',
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }
    } catch (error) {
        // API, transport or SDK error
        console.error(error)
    }
    ```

- PHP

    ```php
    $connectorCode = 'a588e1a88baaf301b9d0b0b33b1eefc2';

    try {
        $response = $b24->core->call('imopenlines.network.join', [
            'CODE' => $connectorCode,
        ]);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- Python

    ```python
    connector_code = "a588e1a88baaf301b9d0b0b33b1eefc2"

    try:
        response = client.imopenlines.network.join(
            code=connector_code,
        ).response
    except BitrixAPIError as error:
        print(f"Open line connection error: {error}")
    ```

{% endlist %}

Successful response:

```json
{
    "result": 123
}
```

The `result` value is the identifier of the network bot that represents the Open Channel in Bitrix24 chats.

## 2. Send a Welcome Message

Pass the same connector code, the User ID, and the text to the [imopenlines.network.message.add](../../api-reference/imopenlines/openlines/imopenlines-network-message-add.md) method. The method does not work with session authorization: in the application handler, use the OAuth token from the event body.

{% list tabs %}

- JS

    ```js
    const connectorCode = 'a588e1a88baaf301b9d0b0b33b1eefc2'
    const userId = Number(auth.user_id)
    const message = 'Thanks for installing! If you have any questions, write to this chat. Have a nice day! :)'

    try {
        const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.network.message.add',
            params: {
                CODE: connectorCode,
                MESSAGE: message,
                USER_ID: userId,
            },
            requestId: 'network-message',
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }
    } catch (error) {
        // API, transport or SDK error
        console.error(error)
    }
    ```

- PHP

    ```php
    $connectorCode = 'a588e1a88baaf301b9d0b0b33b1eefc2';
    $userId = (int)$request->request->all('auth')['user_id'];
    $message = 'Thanks for installing! If you have any questions, write to this chat. Have a nice day! :)';

    try {
        $response = $b24->core->call('imopenlines.network.message.add', [
            'CODE' => $connectorCode,
            'MESSAGE' => $message,
            'USER_ID' => $userId,
        ]);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- Python

    ```python
    connector_code = "a588e1a88baaf301b9d0b0b33b1eefc2"
    user_id = int(auth["user_id"])
    message = "Thanks for installing! If you have any questions, write to this chat. Have a nice day! :)"

    try:
        response = client.imopenlines.network.message.add(
            code=connector_code,
            message=message,
            user_id=user_id,
        ).response
    except BitrixAPIError as error:
        print(f"Message sending error: {error}")
    ```

{% endlist %}

Successful response:

```json
{
    "result": true
}
```

## Verify the Result

1. In the response of the [imopenlines.network.join](../../api-reference/imopenlines/openlines/imopenlines-network-join.md) method, the `result` field must contain the network bot ID.
2. In the response of the [imopenlines.network.message.add](../../api-reference/imopenlines/openlines/imopenlines-network-message-add.md) method, the `result` field must contain the value `true`.
3. The User whose identifier was passed must see the welcome message from the line in the messenger.

## Errors and Diagnostics

If the Open Channel failed to connect or the message was not sent, determine which method returned an error and find its code in the API response or the SDK message.

### Errors for Both Methods

| Error Code    | What to Check and Fix                                                                           |
| ------------- | ----------------------------------------------------------------------------------------------- |
| `CODE`        | Copy the "Code" field value from the connector page again. The code must contain 32 characters. |
| `IMBOT_ERROR` | Contact the administrator: module `imbot` is not installed.                                     |

### Open Channel Fails to Connect

Check the error code of the [imopenlines.network.join](../../api-reference/imopenlines/openlines/imopenlines-network-join.md) method.

| Error Code       | What to Check and Fix                                                                                |
| ---------------- | ---------------------------------------------------------------------------------------------------- |
| `LINE_NOT_FOUND` | In Bitrix24 Helpdesk, ensure that the "Bitrix24 Network" connector is connected to the Open Channel. |
| `INACTIVE`       | In Bitrix24 Helpdesk, ensure that the Open Channel is active.                                        |

After fixing, call the [imopenlines.network.join](../../api-reference/imopenlines/openlines/imopenlines-network-join.md) method again.

### Welcome Message Fails to Send

Check the error code of the [imopenlines.network.message.add](../../api-reference/imopenlines/openlines/imopenlines-network-message-add.md) method.

| Error Code           | What to Check and Fix                                                                                            |
| -------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `WRONG_AUTH_TYPE`    | Use the OAuth token from the application installation data instead of session authorization.                     |
| `NOT_FOUND`          | In Bitrix24 Helpdesk, ensure that the Open Channel is active and the connector is connected.                     |
| `USER_ID_EMPTY`      | Pass the User identifier from `auth[user_id]` in the `USER_ID` parameter.                                        |
| `USER_MESSAGE_LIMIT` | A message has already been sent to this User this week. Retry later or check the scenario with a different User. |
| `MESSAGE_EMPTY`      | Pass non-empty text in the `MESSAGE` parameter.                                                                  |
| `WRONG_REQUEST`      | Check the values of parameters `CODE`, `USER_ID`, and `MESSAGE`.                                                 |

## Continue Learning

- [Connect an external Open Channel to the portal using imopenlines.network.join](../../api-reference/imopenlines/openlines/imopenlines-network-join.md)
- [Send a message to a User on behalf of an Open Channel using imopenlines.network.message.add](../../api-reference/imopenlines/openlines/imopenlines-network-message-add.md)
- [Open Channels: Overview of Methods and Events](../../api-reference/imopenlines/openlines/index.md)
