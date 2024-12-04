# Add Message im.message.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.message.add` sends messages in a chat.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `chat13`
or
`256` | Identifier of the dialog. Format:
- **chatXXX** – chat of the recipient, if the message is for chats
- **XXX** – identifier of the recipient, if the message is for a private dialog | 18 ||
|| **MESSAGE^*^**
[`unknown`](../../data-types.md) | `Message text` | Text of the message | 18 ||
|| **SYSTEM**
[`unknown`](../../data-types.md) | `N` | Display messages as a system message or not, optional field, default is 'N' | 18 ||
|| **ATTACH**
[`unknown`](../../data-types.md) | | Attachment | 18 ||
|| **URL_PREVIEW**
[`unknown`](../../data-types.md) | `Y` | Convert links to rich links, optional field, default is 'Y' | 18 ||
|| **KEYBOARD**
[`unknown`](../../data-types.md) | | Keyboard | 18 ||
|| **MENU**
[`unknown`](../../data-types.md) | | Context menu | 18 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'im.message.add',
        Array(
            'DIALOG_ID' => 'chat13',
            'MESSAGE' => 'Message text',
            'SYSTEM' => 'N',
            'ATTACH' => '',
            'URL_PREVIEW' => 'Y',
            'KEYBOARD' => '',
            'MENU' => '',
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": 11
}
```

**Execution result**: message identifier `MESSAGE_ID` or error.

## Response on Error

```json
{
    "error": "USER_ID_EMPTY",
    "error_description": "Recipient identifier is not specified when sending a message in a one-on-one chat"
}
```

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **USER_ID_EMPTY** | Recipient identifier is not specified when sending a message in a one-on-one chat ||
|| **CHAT_ID_EMPTY** | Chat identifier of the recipient is not specified when sending a message in a chat ||
|| **ACCESS_ERROR** | Insufficient permissions to send the message ||
|| **MESSAGE_EMPTY** | Message text is not provided ||
|| **ATTACH_ERROR** | The entire provided attachment object failed validation ||
|| **ATTACH_OVERSIZE** | The maximum allowable size of the attachment (30 KB) has been exceeded ||
|| **KEYBOARD_ERROR** | The entire provided keyboard object failed validation ||
|| **KEYBOARD_OVERSIZE** | The maximum allowable size of the keyboard (30 KB) has been exceeded ||
|| **MENU_ERROR** | The entire provided menu object failed validation ||
|| **MENU_OVERSIZE** | The maximum allowable size of the menu (30 KB) has been exceeded ||
|| **PARAMS_ERROR** | Something went wrong ||
|#

## Related Links:

- [How to work with input keyboards](.)
- [How to work with attachments](.)
- [Message formatting](.)
- [Working with context menus](.)