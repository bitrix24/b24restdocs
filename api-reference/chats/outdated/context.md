# Contextual Applications

You can read about what contextual applications are [here](./chat-apps.md#contextual-applications).

To work with the context, perform the following actions:

1. [Register an application](./create-app/index.md). You can register a hidden application so that it does not appear on the text input panel.

2. Send (or update) any message [with an attached keyboard](../messages/keyboards.md) or [with a menu](../messages/menu.md).

3. In the button parameters of the keyboard or menu item, you need to pass the application ID.

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => 2,
        "BOT_ID" => 17,
        "MESSAGE" => "Hello! My name is EchoBot :)",
        "KEYBOARD" => [
            {
                "TEXT":"Open App",
                "APP_ID":11
            }
        ],
        "MENU" => [
            {
                "TEXT":"Open App",
                "APP_ID":11
            }
        ]
    ),
    $_REQUEST["auth"]
);
```
{% include [Footnote about examples](../../../_includes/examples.md) %}

{% note info %}

In addition to `APP_ID`, you can pass any string in the `APP_PARAMS` parameter. When your IFRAME is opened, the data will be passed to the `BUTTON_PARAMS` parameter.

The rules for developing the IFRAME handler and limitations are presented [in the documentation](./iframe.md). When creating a message, you can use one of two options — [keyboard (KEYBOARD)](../messages/keyboards.md) or [context menu (MENU)](../messages/menu.md).

{% endnote %}