# Exclude Participants from Chat imbot.chat.user.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.user.delete` excludes participants.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID**
[`unknown`](../../data-types.md) | `13` | Chat identifier | ||
|| **USER_ID**
[`unknown`](../../data-types.md) | `4` | Identifier of the user to be excluded | ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the chat bot making the request. Can be omitted if there is only one chat bot | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.chat.user.delete',
        Array(
            'CHAT_ID' => 13,
            'USER_ID' => 4,
            'BOT_ID' => 39,
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Success Response

`true`.

## Error Response

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided. ||
|| **USER_ID_EMPTY** | User identifier not provided. ||
|| **WRONG_REQUEST** | You do not have sufficient permissions to exclude the participant from the chat or the specified user is not in the chat. ||
|#