# Update Sent Message im.message.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed for writing standards
- parameter types are not specified
- examples are missing
- links to pages that have not yet been created are not provided

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `im.message.update` method sends updates to a chatbot message.

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

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

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

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result": true
}
```

**Execution result**: `true` or an error.

## Error Response

```json
{
    "error": "MESSAGE_ID_ERROR",
    "error_description": "Message identifier not provided"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **MESSAGE_ID_ERROR** | Message identifier not provided ||
|| **CANT_EDIT_MESSAGE** | You do not have access to this message or the time to modify it has expired (more than 3 days have passed since publication) ||
|| **ATTACH_ERROR** | The entire provided attachment object failed validation ||
|| **ATTACH_OVERSIZE** | The maximum allowable attachment size has been exceeded (30 KB) ||
|| **KEYBOARD_ERROR** | The entire provided keyboard object failed validation ||
|| **KEYBOARD_OVERSIZE** | The maximum allowable keyboard size has been exceeded (30 KB) ||
|| **MENU_ERROR** | The entire provided menu object failed validation ||
|| **MENU_OVERSIZE** | The maximum allowable menu size has been exceeded (30 KB) ||
|#

## Related Links:

- [How to work with keyboards](.)
- [How to work with attachments](.)
- [Message formatting](.)