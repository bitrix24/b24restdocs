# Update Sent Message imbot.message.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- not all parameters have examples in the table
- examples are missing
- success response is missing
- error response is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.message.update` sends updates to a chatbot message.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **BOT_ID^*^**
[`unknown`](../../data-types.md) | `39` | Identifier of the chatbot from which the request is made; can be omitted if there is only one bot | ||
|| **MESSAGE_ID^*^**
[`unknown`](../../data-types.md) | `1` | Identifier of the message | ||
|| **MESSAGE**
[`unknown`](../../data-types.md) | `'answer text'` | Text of the message; if an empty value is passed, the message will be deleted | ||
|| **ATTACH**
[`unknown`](../../data-types.md) | `''` | Attachment | ||
|| **KEYBOARD**
[`unknown`](../../data-types.md) | `''` | Keyboard | ||
|| **MENU**
[`unknown`](../../data-types.md) | `''` | Context menu | ||
|| **URL_PREVIEW**
[`unknown`](../../data-types.md) | `'Y'` | Convert links to rich links | ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.message.update',
        Array(
            'BOT_ID' => 39,
            'MESSAGE_ID' => 1,
            'MESSAGE' => 'answer text',
            'ATTACH' => '',
            'KEYBOARD' => '',
            'MENU' => '',
            'URL_PREVIEW' => 'Y'
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Example Note](../../../_includes/examples.md) %}

## Success Response

`true`.

## Error Response

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **BOT_ID_ERROR** | Chatbot not found. ||
|| **APP_ID_ERROR** | The chatbot does not belong to this application. You can only work with chatbots installed within the application. ||
|| **MESSAGE_EMPTY** | No message text was provided. ||
|| **CANT_EDIT_MESSAGE** | You do not have access to this message or the time to modify it has expired (more than 3 days have passed since publication). ||
|| **ATTACH_ERROR** | The entire provided attachment object failed validation. ||
|| **ATTACH_OVERSIZE** | The maximum allowed size for the attachment has been exceeded (30 KB). ||
|| **KEYBOARD_ERROR** | The entire provided keyboard object failed validation. ||
|| **KEYBOARD_OVERSIZE** | The maximum allowed size for the keyboard has been exceeded (30 KB). ||
|| **MENU_ERROR** | The entire provided menu object failed validation. ||
|| **MENU_OVERSIZE** | The maximum allowed size for the menu has been exceeded (30 KB). ||
|#

## Continue Learning

- [How to Work with Keyboards](../../chats/messages/keyboards.md)
- [How to Work with Attachments](../../chats/messages/attachments/index.md)
- [Message Formatting](../../chats/messages/index.md)
- [Event for Receiving a Message by the Chatbot ONIMBOTMESSAGEADD](./events/on-imbot-message-add.md)