# Change Sent Message imbot.message.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- not all parameters have examples in the table
- examples are missing
- response in case of success is missing
- response in case of error is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.message.update` sends changes to a chatbot message.

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

{% include [Parameter Notes](../../../_includes/required.md) %}

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

{% include [Example Notes](../../../_includes/examples.md) %}

## Response in Case of Success

`true`.

## Response in Case of Error

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **BOT_ID_ERROR** | Chatbot not found. ||
|| **APP_ID_ERROR** | Chatbot does not belong to this application. You can only work with chatbots installed within the application. ||
|| **MESSAGE_EMPTY** | Message text not provided. ||
|| **CANT_EDIT_MESSAGE** | You do not have access to this message or the time to modify it has expired (more than 3 days have passed since publication). ||
|| **ATTACH_ERROR** | The entire provided attachment object failed validation. ||
|| **ATTACH_OVERSIZE** | The maximum allowable size for the attachment (30 KB) has been exceeded. ||
|| **KEYBOARD_ERROR** | The entire provided keyboard object failed validation. ||
|| **KEYBOARD_OVERSIZE** | The maximum allowable size for the keyboard (30 KB) has been exceeded. ||
|| **MENU_ERROR** | The entire provided menu object failed validation. ||
|| **MENU_OVERSIZE** | The maximum allowable size for the menu (30 KB) has been exceeded. ||
|#

## Continue Learning

- [How to Work with Keyboards](../../chats/messages/keyboards.md)
- [How to Work with Attachments](../../chats/messages/attachments/index.md)
- [Message Formatting](../../chats/messages/index.md)
- [Event for Receiving a Message by the Chatbot ONIMBOTMESSAGEADD](./events/index.md)