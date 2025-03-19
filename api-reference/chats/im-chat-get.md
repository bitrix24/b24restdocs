# Get Chat ID im.chat.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- response in case of error not provided
- links to yet-to-be-created pages not specified
- from Sergei's file: unclear in what context the method is applicable, needs clarification

{% endnote %}

{% endif %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.get` retrieves the chat ID.

#|
|| **Parameter** | **Example** | **Description** ||
|| **ENTITY_TYPE^*^**
[`unknown`](../data-types.md) | `CRM`, `LINES`, `LIVECHAT` | Entity identifier. Can be used to find the chat and easily determine the context in event handlers:
- [ONIMBOTMESSAGEADD](../chat-bots/messages/events/on-imbot-message-add.md),
- [ONIMBOTMESSAGEUPDATE](../chat-bots/messages/events/on-imbot-message-update.md),
- [ONIMBOTMESSAGEDELETE](../chat-bots/messages/events/on-imbot-message-delete.md) ||
|| **ENTITY_ID^*^**
[`unknown`](../data-types.md) | `LEAD`\|`13` | Numeric entity identifier. Can be used to find the chat and easily determine the context in event handlers:
- [ONIMBOTMESSAGEADD](../chat-bots/messages/events/on-imbot-message-add.md),
- [ONIMBOTMESSAGEUPDATE](../chat-bots/messages/events/on-imbot-message-update.md),
- [ONIMBOTMESSAGEDELETE](../chat-bots/messages/events/on-imbot-message-delete.md) ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Examples

{% include [Explanation about restCommand](./_includes/rest-command.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'im.chat.get',
        {
            ENTITY_TYPE: "LINES",
            ENTITY_ID: "telegrambot|2|209607941|744"
        
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

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

{% include [Footnote about examples](../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": 10
}
```

**Execution result**: returns the chat ID `CHAT_ID` or `null`.