# Invite Participants to Chat im.chat.user.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed for standard writing
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.user.add` invites participants to the chat.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^**
[`unknown`](../../data-types.md) | `13` | Identifier of the chat | 18 ||
|| **USERS^*^**
[`unknown`](../../data-types.md) | `Array(3,4)` | Identifiers of new users | 18 ||
|| **HIDE_HISTORY**
[`unknown`](../../data-types.md) | `N` | Display of chat history for the user being added to the chat by this method. Y/N - defaults to N. If Y is passed, the new user will not see the history. | 18 ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'im.chat.user.add',
        Array(
            'CHAT_ID' => 13,
            'USERS' => Array(3,4),
            'HIDE_HISTORY' => 'N',
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

## Response on Error

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat identifier not provided"
}
```

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided ||
|| **WRONG_REQUEST** | You do not have sufficient permissions to add a participant to the chat or the participant is already in the chat ||
|#