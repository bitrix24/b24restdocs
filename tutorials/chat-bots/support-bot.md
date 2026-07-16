# Example of Creating a Support Channel

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

With the **Open Channels** module, you can organize technical support for any Bitrix24 application, including chatbots.

Interface preparation:

- Go to the **Contact Center** section and connect a communication `Bitrix24.Network` channel
- Fill in `Name`, Short description, and set `Avatar` — this will help customers find you more easily
- Create a new technical support Open Channel or select an existing one

{% note info "" %}

`imopenlines.network.*` methods operate within the context of an [application](../../settings/app-installation/index.md). The application receives authorization (`access_token`, `domain`) in the request body. For SDK initialization, see the [chatbot example](./index.md#initializing-the-sdk-using-event-data).

{% endnote %}

## Connecting an Open Channel to a Portal

The [imopenlines.network.join](../../api-reference/imopenlines/openlines/imopenlines-network-join.md) method automatically connects your Open Channel to the user's portal. The code from the connectors page is passed to the `CODE` parameter.

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'imopenlines.network.join',
        params: { CODE: 'a588e1a88baaf301b9d0b0b33b1eefc2b' },
        requestId: 'network-join',
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }
    ```

- PHP

    ```php
    // Typed analog: $b24->getIMOpenLinesScope()->Network()->join(...)
    $b24->core->call('imopenlines.network.join', [
        'CODE' => 'a588e1a88baaf301b9d0b0b33b1eefc2b',
    ]);
    ```

- Python

    ```python
    client.imopenlines.network.join(
        code="a588e1a88baaf301b9d0b0b33b1eefc2b",
    ).response
    ```

{% endlist %}

## Welcome Message

After connecting, send a greeting to the customer using the [imopenlines.network.message.add](../../api-reference/imopenlines/openlines/imopenlines-network-message-add.md) method.

{% list tabs %}

- JS

    ```js
    await $b24.actions.v2.call.make({
        method: 'imopenlines.network.message.add',
        params: {
            CODE: 'a588e1a88baaf301b9d0b0b33b1eefc2b',
            MESSAGE: 'Thanks for installing! If you have any questions, write to this chat. Have a nice day! :)',
            USER_ID: userId,
        },
        requestId: 'network-message',
    })
    ```

- PHP

    ```php
    $b24->core->call('imopenlines.network.message.add', [
        'CODE' => 'a588e1a88baaf301b9d0b0b33b1eefc2b',
        'MESSAGE' => 'Thanks for installing! If you have any questions, write to this chat. Have a nice day! :)',
        'USER_ID' => $userId,
    ]);
    ```

- Python

    ```python
    client.imopenlines.network.message.add(
        code="a588e1a88baaf301b9d0b0b33b1eefc2b",
        message="Thanks for installing! If you have any questions, write to this chat. Have a nice day! :)",
        user_id=user_id,
    ).response
    ```

{% endlist %}

{% note tip "" %}

A support channel can also be opened from the frontend of an iframe application via b24jssdk: `await $b24.parent.imOpenMessenger(dialogId)` — the equivalent of the `BX24.im.openMessenger` method.

{% endnote %}
