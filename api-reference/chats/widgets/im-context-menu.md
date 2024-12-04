# Widget as a Context Menu Item in IM_CONTEXT_MENU

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- from Sergei's file: general description, for which scenarios it is useful, link to the page in the widget section

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)

Embedding in the "Create content based on" item (analogous to "Create task" or "Create meeting" based on a message).

#|
|| **Parameter** | **Description** ||
|| **context**
[`unknown`](../../data-types.md) | For which type of chat to embed the application (default is `ALL`). Supports multiple selections via `;` of the following values: 
- **USER** – chats of all users, excluding bots;
- **CHAT** – all group chats, except lines and crm;
- **LINES** – lines chat type (open lines);
- **CRM** – only chats created within CRM;
- **ALL** – all chats.
 ||
|| **role**
[`unknown`](../../data-types.md) | User role for which this application is available (default is `USER`). Supports values: 
- **USER** – application available to all users;
- **ADMIN** – application available only to portal administrators.
 ||
|| **extranet**
[`unknown`](../../data-types.md) | Is the application available for extranet users (default is `N`). Supports values:
- **N** – application not available for extranet users;
- **Y** – application available for extranet users.
 ||
|#

In this embedding, the current opening context is available, and `dialogId` of the current chat and `messageId` of the selected message will be passed.

```js
const context = BX24.placement.info().options;
```

The application will open in a slider style (like tasks or calendar).

## Examples

{% list tabs %}

- PHP

    ```php
    CRest::call(
        'placement.bind',
        [
            'PLACEMENT' => 'IM_CONTEXT_MENU',
            'HANDLER' => 'https://example.com/apps/immarket/handlers/context_menu.php',
            'LANG_ALL' => [
                'de' => [
                    'TITLE' => 'Application to open the context menu of a message within the chat',
                ],
            ],
            'OPTIONS' => [
                'context' => 'USER;CHAT',
                'role' => 'USER',
                'extranet' => 'N',
            ]
        ]
    );
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}