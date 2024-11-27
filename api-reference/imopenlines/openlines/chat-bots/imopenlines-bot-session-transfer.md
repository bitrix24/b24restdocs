# Switch the dialog to an operator by Id imopenlines.bot.session.transfer

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines, imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method switches the conversation to a specific operator.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Example** | **Description** | **Revision** ||
|| **CHAT_ID*** 
[`unknown`](../../../data-types.md) | `112` | Identifier of the chat | 1 ||
|| **USER_ID*** 
[`unknown`](../../../data-types.md) | `12` | Identifier of the user to whom the conversation is redirected | 1 ||
|| **LEAVE*** 
[`unknown`](../../../data-types.md) | `N` | Y/N. If N is specified — the chatbot will not leave this chat after redirection and will remain until the user confirms | 1 ||
|#

{% note info %}

Instead of `USER_ID`, you can specify `QUEUE_ID` to switch to another open line.

{% endnote %}

## Examples

{% include [Explanation about restCommand](../../../chat-bots/_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imopenlines.bot.session.transfer',
        Array(
            'CHAT_ID' => 112,
            'USER_ID' => 12,
            'LEAVE' => 'N'
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

{% include [Note on examples](../../../../_includes/examples.md) %}

## Response on success

```json
{
    "result": true
}
```

## Response on error

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat identifier not provided"
}
```

### Description of keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided ||
|| **USER_ID_EMPTY** | User identifier to whom the conversation needs to be redirected is not provided ||
|| **WRONG_CHAT** | Incorrect user identifier specified or this user is a chatbot or extranet user ||
|| **BOT_ID_ERROR** | Incorrect chatbot identifier ||
|#