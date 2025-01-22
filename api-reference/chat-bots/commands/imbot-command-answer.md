# Send a response to the command imbot.command.answer

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards
- parameter types are not specified
- parameter requirements are not indicated
- not all parameters have examples in the table
- examples are missing
- no response in case of success
- no response in case of error
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.command.answer` publishes a response to the command.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **COMMAND_ID**
[`unknown`](../../data-types.md) | `13` | Identifier of the command that prepared the response (must specify either COMMAND or COMMAND_ID) | ||
|| **COMMAND**
[`unknown`](../../data-types.md) | `'echo'` | Name of the command that prepared the response (must specify either COMMAND_ID or COMMAND) | ||
|| **MESSAGE_ID**
[`unknown`](../../data-types.md) | `1122` | Identifier of the message to which a response is required | ||
|| **MESSAGE**
[`unknown`](../../data-types.md) | `'answer text'` | Text of the response | ||
|| **ATTACH**
[`unknown`](../../data-types.md) | `''` | Attachment, optional field | ||
|| **KEYBOARD**
[`unknown`](../../data-types.md) | `''` | Keyboard, optional field | ||
|| **MENU**
[`unknown`](../../data-types.md) | `''` | Context menu, optional field | ||
|| **SYSTEM**
[`unknown`](../../data-types.md) | `'N'` | Display messages as system messages, optional field, default is 'N' | ||
|| **URL_PREVIEW**
[`unknown`](../../data-types.md) | `'Y'` | Convert links to rich links, optional field, default is 'Y' | ||
|| **CLIENT_ID**
[`unknown`](../../data-types.md) | `''` | String identifier of the chatbot, used only in Webhook mode | ||
|#

{% note warning %}

To process the command, the application must handle the event of adding a command [ONIMCOMMANDADD](./events/on-im-command-add.md).

{% endnote %}

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.command.answer',
        Array(
            'COMMAND_ID' => 13,
            'COMMAND' => 'echo',
            'MESSAGE_ID' => 1122,
            'MESSAGE' => 'answer text',
            'ATTACH' => '',
            'KEYBOARD' => '',
            'MENU' => '',
            'SYSTEM' => 'N',
            'URL_PREVIEW' => 'Y',
            'CLIENT_ID' => '',
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response in case of success

Identifier of the command message `MESSAGE_ID`.

## Response in case of error

error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **COMMAND_ID_ERROR** | Command not found. ||
|| **APP_ID_ERROR** | The chatbot does not belong to this application. You can only work with chatbots installed within the application. ||
|| **MESSAGE_EMPTY** | No message text provided. ||
|| **ATTACH_ERROR** | The entire provided attachment object failed validation. ||
|| **ATTACH_OVERSIZE** | The maximum allowable size for the attachment has been exceeded (30 KB). ||
|| **KEYBOARD_ERROR** | The entire provided keyboard object failed validation. ||
|| **KEYBOARD_OVERSIZE** | The maximum allowable size for the keyboard has been exceeded (30 KB). ||
|| **MENU_ERROR** | The entire provided menu object failed validation. ||
|| **MENU_OVERSIZE** | The maximum allowable size for the menu has been exceeded (30 KB). ||
|| **WRONG_REQUEST** | Something went wrong. ||
|#

## Related Links

- [How to work with input keyboards](../../chats/messages/keyboards.md)
- [How to work with attachments](../../chats/messages/attachments/index.md)
- [Message formatting](../../chats/messages/index.md)