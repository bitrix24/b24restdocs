# Chat-bot exit from the specified chat imbot.chat.leave

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.leave` allows a chat-bot to exit the specified chat.

#| 
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID** 
[`unknown`](../../data-types.md) | `13` | The identifier of the chat to exit from | ||
|| **BOT_ID** 
[`unknown`](../../data-types.md) | `39` | The identifier of the chat-bot making the request. It can be omitted if there is only one chat-bot | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.chat.leave',
        Array(
            'CHAT_ID' => 13,
            'BOT_ID' => 39,
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response in case of success

`true`.

## Response in case of error

error

### Possible error codes

#| 
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | The chat identifier was not provided. ||
|| **ACCESS_ERROR** | You do not have sufficient permissions to send the status to this chat. ||
|| **BOT_ID_ERROR** | The chat-bot was not found. ||
|#