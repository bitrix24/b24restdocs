# Widget in the IM_SMILES_SELECTOR Tool

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

There may be custom sources for images or emojis.

#|
|| Parameter | Description ||
|| **context**
[`unknown`](../../data-types.md) | For which type of chat to embed the application (default is `ALL`). Supports multiple selections through `;` of the following values: 
- **USER** – chats of all users, excluding bots;
- **CHAT** – all group chats, except lines and crm;
- **LINES** – type of chat lines (open lines);
- **CRM** – only chats created within CRM;
- **ALL** – all chats.
 ||
|| **role**
[`unknown`](../../data-types.md) | User role for which this application is available (default is `USER`). Supports values:
- **USER** – application is available to all users;
- **ADMIN** – application is available only to portal administrators.
 ||
|| **extranet**
[`unknown`](../../data-types.md) | Is the application available for extranet users (default is `N`). Supports values:
- **N** – application is not available for extranet users;
- **Y** – application is available for extranet users.
 ||
|#

In this embedding, the current opening context is available, and the `dialogId` of the current chat will be passed.

```js
const context = BX24.placement.info().options;
```

The application is rendered within a pop-up window with an emoji and Giphy selector (without the ability to resize).

## Examples

{% list tabs %}

- PHP

    ```php
    CRest::call(
        'placement.bind',
        [
            'PLACEMENT' => 'IM_SMILES_SELECTOR',
            'HANDLER' => 'https://example.com/apps/immarket/handlers/smiles_selector.php',
            'LANG_ALL' => [
                'de' => [
                    'TITLE' => 'Application to enhance emoji and Giphy capabilities',
                ],
            ],
            'OPTIONS' => [
                'context' => 'USER;LINES',
                'role' => 'USER',
                'extranet' => 'Y',
            ]
        ]
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}