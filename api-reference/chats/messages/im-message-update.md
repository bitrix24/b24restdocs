# Update Sent Message im.message.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- links to pages that have not yet been created are not provided

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.message.update` sends updates to a chatbot message.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **MESSAGE_ID^*^**
[`unknown`](../../data-types.md) | `1` | Message identifier | 18 ||
|| **MESSAGE**
[`unknown`](../../data-types.md) | `Message text` | The text of the message. If an empty value is passed, the message will be deleted | 18 ||
|| **ATTACH**
[`unknown`](../../data-types.md) | | Attachment | 18 ||
|| **URL_PREVIEW**
[`unknown`](../../data-types.md) | `Y` | Convert links to rich links | 18 ||
|| **KEYBOARD**
[`unknown`](../../data-types.md) | | Keyboard | 18 ||
|| **MENU**
[`unknown`](../../data-types.md) | | Context menu | 18 ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand('
    im.message.update',
    Array(
        'MESSAGE_ID' => 1,
        'MESSAGE' => 'Message text',
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

{% include [Examples Note](../../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": true
}
```

**Execution result**: `true` or an error.

## Response on Error

```json
{
    "error": "MESSAGE_ID_ERROR",
    "error_description": "Message identifier not provided"
}
```

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **MESSAGE_ID_ERROR** | Message identifier not provided ||
|| **CANT_EDIT_MESSAGE** | You do not have access to this message or the time for modification has expired (more than 3 days have passed since publication) ||
|| **ATTACH_ERROR** | The entire provided attachment object failed validation ||
|| **ATTACH_OVERSIZE** | The maximum allowable size for the attachment has been exceeded (30 KB) ||
|| **KEYBOARD_ERROR** | The entire provided keyboard object failed validation ||
|| **KEYBOARD_OVERSIZE** | The maximum allowable size for the keyboard has been exceeded (30 KB) ||
|| **MENU_ERROR** | The entire provided menu object failed validation ||
|| **MENU_OVERSIZE** | The maximum allowable size for the menu has been exceeded (30 KB) ||
|#

## Related Links:

- [How to work with input keyboards](.)
- [How to work with attachments](.)
- [Message formatting](.)