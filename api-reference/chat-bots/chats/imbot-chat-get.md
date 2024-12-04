# Get Chat ID imbot.chat.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- examples missing
- success response missing
- error response missing
- links to pages not yet created not specified (make links to events)
- from Sergei's file: unclear in what context the method is applicable, needs clarification

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.chat.get` retrieves the chat ID.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ENTITY_TYPE^*^**
[`unknown`](../../data-types.md) | `'OPEN'` | Identifier of an arbitrary entity (e.g., CHAT, CRM, OPENLINES, CALL, etc.), can be used to find the chat and easily determine the context in event handlers ONIMBOTMESSAGEADD, ONIMBOTMESSAGEUPDATE, ONIMBOTMESSAGEDELETE | ||
|| **ENTITY_ID^*^**
[`unknown`](../../data-types.md) | `13` | Numeric identifier of the entity, can be used to find the chat and easily determine the context in event handlers ONIMBOTMESSAGEADD, ONIMBOTMESSAGEUPDATE, ONIMBOTMESSAGEDELETE | ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the chat bot making the request. Can be omitted if there is only one chat bot | ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.chat.get',
        Array(
            'ENTITY_TYPE' => 'OPEN',
            'ENTITY_ID' => 13,
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

Returns the chat ID `CHAT_ID` or `null`.