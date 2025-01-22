# Send Message imbot.message.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing for some parameters in the table
- examples are absent
- success response is missing
- error response is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.message.add` sends a message from the chat bot.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **BOT_ID^*^**
[`unknown`](../../data-types.md) | `39` | Identifier of the chat bot from which the request is made; can be omitted if there is only one chat bot | ||
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
[`unknown`](../../data-types.md) | `'N'` | Display messages as system messages, default is 'N' | ||
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

{% include [Example Notes](../../../_includes/examples.md) %}

You can publish a message on behalf of the bot in private dialogs (it will be shown as a system message). To do this, instead of `DIALOG_ID`, specify `FROM_USER_ID` and `TO_USER_ID`.

## Success Response

Message identifier `MESSAGE_ID`.

## Error Response

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **BOT_ID_ERROR** | Chat bot not found. ||
|| **APP_ID_ERROR** | The chat bot does not belong to this application. You can only work with chat bots installed within the application. ||
|| **DIALOG_ID_EMPTY** | Dialog identifier not provided. ||
|| **MESSAGE_EMPTY** | Message text not provided. ||
|| **ATTACH_ERROR** | The entire provided attachment object failed validation. ||
|| **ATTACH_OVERSIZE** | The maximum allowable size for the attachment (30 KB) has been exceeded. ||
|| **KEYBOARD_ERROR** | The entire provided keyboard object failed validation. ||
|| **KEYBOARD_OVERSIZE** | The maximum allowable size for the keyboard (30 KB) has been exceeded. ||
|| **MENU_ERROR** | The entire provided menu object failed validation. ||
|| **MENU_OVERSIZE** | The maximum allowable size for the menu (30 KB) has been exceeded. ||
|| **WRONG_REQUEST** | Something went wrong. ||
|#

## Continue Learning

- [How to Work with Keyboards](../../chats/messages/keyboards.md)
- [How to Work with Attachments](../../chats/messages/attachments/index.md)
- [Message Formatting](../../chats/messages/index.md)
- [Event for Receiving Messages by the Chat Bot ONIMBOTMESSAGEADD](./events/on-imbot-message-add.md)