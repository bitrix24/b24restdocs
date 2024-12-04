# Widget for the message input field IM_TEXTAREA

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- from Sergei's file: general description, for which scenarios it is useful, link to the page in the widget section

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)

This format has been around before – it generates content at the moment of message writing.

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
[`unknown`](../../data-types.md) | The role of the user for which this application is available (default is `USER`). Supports values:
- **USER** – application available to all users;
- **ADMIN** – application available only to portal administrators.
 ||
|| **color**
[`unknown`](../../data-types.md) | Color. Available values: `RED`, `GREEN`, `MINT`, `LIGHT_BLUE`, `DARK_BLUE`, `PURPLE`, `AQUA`, `PINK`, `LIME`, `BROWN`, `AZURE`, `KHAKI`, `SAND`, `ORANGE`, `MARENGO`, `GRAY`, `GRAPHITE`. ||
|| **width**
[`unknown`](../../data-types.md) | Recommended width of the frame as in the old chat (default is `100`). ||
|| **height**
[`unknown`](../../data-types.md) | Recommended height of the frame as in the old chat (default is `100`). ||
|| **extranet**
[`unknown`](../../data-types.md) | Is the application available for extranet users (default is `N`). Supports the following values:
- **N** – application not available for extranet users;
- **Y** – application available for extranet users.
 ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

In this embedding, the current opening context is available, and the `dialogId` of the current chat will be passed.

```js
const context = BX24.placement.info().options;
```

An IFRAME of the application will be opened with the specified dimensions. If you specify a size for the application larger than what can be displayed, that size will be automatically reduced (your application should account for this).

## Examples

{% list tabs %}

- PHP

    ```php
    CRest::call(
        'placement.bind',
        [
            'PLACEMENT' => 'IM_TEXTAREA',
            'HANDLER' => 'https://example.com/apps/immarket/handlers/textarea.php',
            'LANG_ALL' => [
                'de' => [
                    'TITLE' => 'Application for the input field panel',
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

{% endlist %}

{% include [Example notes](../../../_includes/examples.md) %}