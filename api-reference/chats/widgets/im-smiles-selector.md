# Widget in the IM_SMILES_SELECTOR Tool

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- from Sergei's file: general description, useful scenarios, link to the page in the widget section

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)

There may be specific sources for images or smiles.

#|
|| Parameter | Description ||
|| **context**
[`unknown`](../../data-types.md) | The type of chat for which to embed the application (default is `ALL`). Supports multiple selections via `;` of the following values: 
- **USER** – chats of all users, excluding bots;
- **CHAT** – all group chats, except lines and crm;
- **LINES** – lines chat type (open lines);
- **CRM** – only chats created within CRM;
- **ALL** – all chats.
 ||
|| **role**
[`unknown`](../../data-types.md) | The user role for which this application is available (default is `USER`). Supports values:
- **USER** – the application is available to all users;
- **ADMIN** – the application is available only to portal administrators.
 ||
|| **extranet**
[`unknown`](../../data-types.md) | Is the application available for extranet users (default is `N`). Supports values:
- **N** – the application is not available for extranet users;
- **Y** – the application is available for extranet users.
 ||
|#

In this embedding, the current opening context is available, and the `dialogId` of the current chat will be passed.

```js
const context = BX24.placement.info().options;
```

The application is rendered within a pop-up window with a smiles and giphy selector (without the ability to resize).

## Examples

```php
CRest::call(
    'placement.bind',
    [
        'PLACEMENT' => 'IM_SMILES_SELECTOR',
        'HANDLER' => 'https://example.com/apps/immarket/handlers/smiles_selector.php',
        'LANG_ALL' => [
            'en' => [
                'TITLE' => 'Application for enhancing smiles and giphy capabilities',
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

{% include [Footnote on examples](../../../_includes/examples.md) %}