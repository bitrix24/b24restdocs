# Send Message imbot.message.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- not all parameters have examples in the table
- examples are missing
- response on success is missing
- response on error is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.message.add` sends a message from the chatbot.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **BOT_ID^*^**
[`unknown`](../../data-types.md) | `39` | Identifier of the chatbot from which the request is made; can be omitted if there is only one chatbot | ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `1` | Identifier of the dialog; this is either the USER_ID of the user or chatXX - where XX is the chat identifier, passed in the ONIMBOTMESSAGEADD and ONIMJOINCHAT events | ||
|| **MESSAGE^*^**
[`unknown`](../../data-types.md) | `'answer text'` | Text of the message | ||
|| **ATTACH**
[`unknown`](../../data-types.md) | `''` | Attachment | ||
|| **KEYBOARD**
[`unknown`](../../data-types.md) | `''` | Keyboard | ||
|| **MENU**
[`unknown`](../../data-types.md) | `''` | Context menu | ||
|| **SYSTEM**
[`unknown`](../../data-types.md) | `'N'` | Display messages as a system message, default is 'N' | ||
|| **URL_PREVIEW**
[`unknown`](../../data-types.md) | `'Y'` | Convert links to rich links, default is 'Y' | ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.message.add',
        Array(
            'BOT_ID' => 39,
            'DIALOG_ID' => 1,
            'MESSAGE' => 'answer text',
            'ATTACH' => '',
            'KEYBOARD' => '',
            'MENU' => '',
            'SYSTEM' => 'N',
            'URL_PREVIEW' => 'Y'
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}

You can publish a message on behalf of the bot in private dialogs (it will be shown as a system message). To do this, instead of `DIALOG_ID`, specify `FROM_USER_ID` and `TO_USER_ID`.

## Response on Success

Message identifier `MESSAGE_ID`.

## Response on Error

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **BOT_ID_ERROR** | Chatbot not found. ||
|| **APP_ID_ERROR** | Chatbot does not belong to this application. You can only work with chatbots installed within the application. ||
|| **DIALOG_ID_EMPTY** | Dialog identifier not provided. ||
|| **MESSAGE_EMPTY** | Message text not provided. ||
|| **ATTACH_ERROR** | The entire provided attachment object failed validation. ||
|| **ATTACH_OVERSIZE** | Exceeded maximum allowed attachment size (30 KB). ||
|| **KEYBOARD_ERROR** | The entire provided keyboard object failed validation. ||
|| **KEYBOARD_OVERSIZE** | Exceeded maximum allowed keyboard size (30 KB). ||
|| **MENU_ERROR** | The entire provided menu object failed validation. ||
|| **MENU_OVERSIZE** | Exceeded maximum allowed menu size (30 KB). ||
|| **WRONG_REQUEST** | Something went wrong. ||
|#

## Continue Learning

- [How to Work with Keyboards](../../chats/messages/keyboards.md)
- [How to Work with Attachments](../../chats/messages/attachments/index.md)
- [Message Formatting](../../chats/messages/index.md)
- [Event for Receiving a Message by the Chatbot ONIMBOTMESSAGEADD](./events/index.md)