# Widget in the IM_SMILES_SELECTOR Tool

{% note warning " " %}

Starting from version `im 25.1600.0`, the `IM_SMILES_SELECTOR` integration is no longer functional. Smiles have been replaced with [stickers](https://helpdesk.bitrix24.com/open/25866875/).

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)

Here you can have your own sources of images or smiles.

#|
|| Parameter | Description ||
|| **context**
[`unknown`](../../data-types.md) | For which type of chat to embed the application (default is `ALL`). Supports multiple selections through `;` of the following values: 
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

In this integration, the current opening context is available, and the `dialogId` of the current chat will be passed.

```js
const context = BX24.placement.info().options;
```

The application is rendered within a pop-up window with a smile selector and giphy (without the ability to resize).

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
                    'TITLE' => 'Application to enhance smiles and giphy capabilities',
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