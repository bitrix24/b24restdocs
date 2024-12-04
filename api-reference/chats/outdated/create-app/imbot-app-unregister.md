# Unregister Chat Application imbot.app.unregister

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