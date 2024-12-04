# Deleting a Chat Bot Message im.message.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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

The method `im.message.delete` retrieves information about the dialog.

#| 
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **MESSAGE_ID^*^**
[`unknown`](../../data-types.md) | `1` | Message identifier | 18 ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'im.message.delete',
        Array(
            'MESSAGE_ID' => 1,
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

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

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| **MESSAGE_ID_ERROR** | Message identifier not provided ||
|| **CANT_EDIT_MESSAGE** | You do not have access to this message ||
|#