# Chat Sidebar Widget IM_SIDEBAR

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- from Sergey's file: general description, useful scenarios, link to the page in the widgets section

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)

You can create applications that add additional scenarios for chat – for example, a separate disk for chat or a knowledge base.

#|
|| **Parameter** | **Description** ||
|| **iconName^*^**
[`unknown`](../../data-types.md) | The class name of the icon in [Font Awesome](https://fontawesome.com/search) format (e.g., `fa-cloud`). ||
|| **context**
[`unknown`](../../data-types.md) | The type of chat to embed the application in (default is `ALL`). Supports multiple selections via `;` of the following values:
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
|| **color**
[`unknown`](../../data-types.md) | Color. Available values: `RED`, `GREEN`, `MINT`, `LIGHT_BLUE`, `DARK_BLUE`, `PURPLE`, `AQUA`, `PINK`, `LIME`, `BROWN`, `AZURE`, `KHAKI`, `SAND`, `ORANGE`, `MARENGO`, `GRAY`, `GRAPHITE`. ||
|| **extranet**
[`unknown`](../../data-types.md) | Is the application available for extranet users (default is `N`). Supports values:
- **N** – the application is not available for extranet users;
- **Y** – the application is available for extranet users.
 ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

In this embedding, the current opening context is available, and the `dialogId` of the current chat will be passed.

```js
const context = BX24.placement.info().options;
```

The application will mimic the sidebar workflow scenario (a slider will be opened, replicating the sidebar detail layer).

## Examples

```php
CRest::call(
    'placement.bind',
    [
        'PLACEMENT' => 'IM_SIDEBAR',
        'HANDLER' => 'https://example.com/apps/immarket/handlers/sidebar.php',
        'LANG_ALL' => [
            'en' => [
                'TITLE' => 'Sidebar Application',
            ],
        ],
        'OPTIONS' => [
            'iconName' => 'fa-bug',
            'context' => 'USER;LINES',
            'role' => 'ADMIN',
            'color' => 'AQUA',
            'extranet' => 'N',
        ]
    ]
);
```

{% include [Footnote about examples](../../../_includes/examples.md) %}