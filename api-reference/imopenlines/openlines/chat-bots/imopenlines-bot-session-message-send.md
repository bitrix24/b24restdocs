# Send a welcome message imopenlines.bot.session.message.send

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for the description standard
- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`imopenlines, imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method sends an automatic message via the chatbot.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^**
[`unknown`](../../../data-types.md) | `2` | Identifier of the chat that the current operator is taking | 1 ||
|| **MESSAGE^*^**
[`unknown`](../../../data-types.md) | `message text` | The message being sent | 1 ||
|| **NAME^*^**
[`unknown`](../../../data-types.md) | `WELCOME` | Type of message:
- `WELCOME` — welcome message
- `DEFAULT` — regular message

By default, empty value | 1 ||
|#

## Examples

{% include [Explanation of restCommand](../../../chat-bots/_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imopenlines.bot.session.message.send',
        Array(
            'CHAT_ID' => 2,
            'MESSAGE' => 'message text',
            'NAME' => 'WELCOME'
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

{% include [Note on examples](../../../../_includes/examples.md) %}

## Response in case of success

```json
{
    "result": true
}
```

## Response in case of error

or an error.