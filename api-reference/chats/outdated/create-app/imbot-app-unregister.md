# Unregister Chat Application imbot.app.unregister

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.app.unregister` removes the application from the chat.

## Method Parameters

#|
|| **Name** | **Example** | **Description** ||
|| **APP_ID** | `13` | Identifier of the application to be removed ||
|#

## Code Example

{% include [Explanation about restCommand](../../_includes/rest-command.md) %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.app.unregister',
        Array(
            'APP_ID' => 13,
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

## Response on Success

`true`

## Possible Error Codes

#|
|| **Code** | **Description** ||
|| `CHAT_APP_ID_ERROR` | Application not found ||
|| `APP_ID_ERROR` | The chat application does not belong to this rest application. Only chat applications installed within the current rest application can be used ||
|| `WRONG_REQUEST` | Something went wrong ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-app-register.md)
- [{#T}](./imbot-app-update.md)