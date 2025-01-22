# Get Chat Identifier im.chat.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- response in case of error is absent
- links to pages that have not yet been created are not provided
- from Sergei's file: it's unclear in what context the method is applicable, needs clarification

{% endnote %}

{% endif %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.get` retrieves the chat identifier.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ENTITY_TYPE^*^**
[`unknown`](../data-types.md) | `CRM` | Identifier of the entity. Can be used to find the chat and to easily determine the context in event handlers:
- [ONIMBOTMESSAGEADD](../chat-bots/messages/events/on-imbot-message-add.md),
- [ONIMBOTMESSAGEUPDATE](../chat-bots/messages/events/on-imbot-message-update.md),
- [ONIMBOTMESSAGEDELETE](../chat-bots/messages/events/on-imbot-message-delete.md) | 18 ||
|| **ENTITY_ID^*^**
[`unknown`](../data-types.md) | `LEAD`\|`13` | Numeric identifier of the entity. Can be used to find the chat and to easily determine the context in event handlers:
- [ONIMBOTMESSAGEADD](../chat-bots/messages/events/on-imbot-message-add.md),
- [ONIMBOTMESSAGEUPDATE](../chat-bots/messages/events/on-imbot-message-update.md),
- [ONIMBOTMESSAGEDELETE](../chat-bots/messages/events/on-imbot-message-delete.md) | 18 ||
|#

{% include [Parameter Note](../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](./_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'im.chat.get',
        Array(
            'ENTITY_TYPE' => 'CRM',
            'ENTITY_ID' => 'LEAD|13',
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Examples Note](../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": 10
}
```

**Execution Result**: returns the chat identifier `CHAT_ID` or `null`.