# General Information on All Embedding Points

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing

{% endnote %}

{% endif %}

All embeddings are in a standard format and are registered using the method [placement.bind](../../widgets/placement-bind.md). Example:

{% list tabs %}

- PHP

    ```php
    CRest::call(
        'placement.bind',
        [
            'PLACEMENT' => 'IM_SIDEBAR',
            'HANDLER' => 'https://example.com/apps/immarket/handlers/sidebar.php',
            'LANG_ALL' => [
                'de' => [
                    'TITLE' => 'Application in the sidebar',
                ],
            ],
            'OPTIONS' => [
                'iconName' => 'fa-bug',
                'context' => 'USER;LINES',
                'role' => 'ADMIN',
                'extranet' => 'N',
            ]
        ]
    );
    ```

{% endlist %}

Let's take a closer look at the `OPTIONS` section:

- `iconName` – the application icon (this is the code from the [Font Awesome](https://fontawesome.com) 6.0 library). The icon is displayed everywhere except in the context menu and emoji selector;
- `context` – the context in which the application is rendered. It is possible to show the application only for certain types of chats (by default, it is active for all types of chats);

{% note info %}

The `ALL` option has the highest priority over all other options, so there is no point in listing other chat types along with it. If this happens, the values of specific chat types will be ignored. However, an incorrect value for one of the passed options will still result in an error when registering the application.

{% endnote %}

- `role` – the user role for which this application will be displayed (by default, `USER` – available to everyone);
- `extranet` – whether the application is available for [extranet users](https://helpdesk.bitrix24.com/open/7215253/) (by default, no).