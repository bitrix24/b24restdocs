# Widget for the Panel Above the IM_TEXTAREA Input Field

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- from Sergei's file: general description, scenarios where it's useful, link to the page in the widgets section

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)

This format has existed before – it generates content at the moment of message writing.

#|
|| **Parameter** | **Description** ||
|| **iconName^*^**
[`unknown`](../../data-types.md) | The class name of the icon in [Font Awesome](https://fontawesome.com/search) format (e.g., `fa-cloud`). ||
|| **context**
[`unknown`](../../data-types.md) | For which type of chat to embed the application (default is `ALL`). Supports multiple selections through `;` of the following values:
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
|| **width**
[`unknown`](../../data-types.md) | Recommended frame width as in the old chat (default is `100`). ||
|| **height**
[`unknown`](../../data-types.md) | Recommended frame height as in the old chat (default is `100`). ||
|| **extranet**
[`unknown`](../../data-types.md) | Is the application available for extranet users (default is `N`). Supports the following values:
- **N** – the application is not available for extranet users;
- **Y** – the application is available for extranet users.
 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

In this embedding, the current opening context is available, and the `dialogId` of the current chat will be passed.

```js
const context = BX24.placement.info().options;
```

An IFRAME of the application will be opened with the specified dimensions. If you specify a size for the application that is larger than can be displayed, that size will be automatically reduced (your application should account for this).

## Examples

```php
CRest::call(
    'placement.bind',
    [
        'PLACEMENT' => 'IM_TEXTAREA',
        'HANDLER' => 'https://example.com/apps/immarket/handlers/textarea.php',
        'LANG_ALL' => [
            'en' => [
                'TITLE' => 'Application for the Panel Above the Input Field',
            ],
        ],
        'OPTIONS' => [
            'iconName' => 'fa-bars',
            'context' => 'USER;CHAT',
            'role' => 'USER',
            'color' => 'GRAPHITE',
            'width' => '200',
            'height' => '100',
            'extranet' => 'N',
        ]
    ]
);
```

{% include [Example Notes](../../../_includes/examples.md) %}