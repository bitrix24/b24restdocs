# Exclude Participants from the Chat imbot.chat.user.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.user.delete` excludes participants.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID**
[`unknown`](../../data-types.md) | `13` | Identifier of the chat | ||
|| **USER_ID**
[`unknown`](../../data-types.md) | `4` | Identifier of the user to be excluded | ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the chatbot making the request. It can be omitted if there is only one chatbot | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

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

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Success Response

`true`.

## Error Response

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier was not provided. ||
|| **USER_ID_EMPTY** | User identifier was not provided. ||
|| **WRONG_REQUEST** | You do not have sufficient permissions to exclude the participant from the chat or the specified user is not in the chat. ||
|#