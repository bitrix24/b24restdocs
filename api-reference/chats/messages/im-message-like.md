# Change the "Like" status for the message im.message.like

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `im.message.like` method sets a "Like" for a message.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **MESSAGE_ID^*^**
[`unknown`](../../data-types.md) | `1` | Identifier of the message (any message sent in personal chats or in group chats where a chatbot is present) | 18 ||
|| **ACTION**
[`unknown`](../../data-types.md) | `auto` | Action related to the method: plus - will set the "Like" label; minus - will remove the "Like" label; auto - will automatically determine whether to set or remove the label. If not specified, it will operate in auto mode | 18 ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'im.message.like',
        Array(
            'MESSAGE_ID' => 1,
            'ACTION' => 'auto',
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Example notes](../../../_includes/examples.md) %}

## Response on success

```json
{
    "result": true
}
```

**Execution result**: `true` or an error.

## Response on error

```json
{
    "error": "MESSAGE_ID_ERROR",
    "error_description": "Message identifier not provided"
}
```

### Description of keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **MESSAGE_ID_ERROR** | Message identifier not provided ||
|| **WITHOUT_CHANGES** | The "Like" status did not change after the call ||
|#