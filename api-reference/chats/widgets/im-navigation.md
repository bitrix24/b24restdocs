# Widget in the Left Menu of the Chat IM_NAVIGATION

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- from Sergey's file: general description, for which scenarios it is useful, link to the page in the widget section

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)

Essentially, this is an application within the chat environment without being embedded directly in the chat.

#|
|| **Parameter** | **Description** ||
|| **iconName^*^**
[`unknown`](../../data-types.md) | The class name of the icon in [Font Awesome](https://fontawesome.com/search) format (e.g., `fa-cloud`). ||
|| **role**
[`unknown`](../../data-types.md) | The user role for which this application is available (default is `USER`). Supports the following access values:
- **USER** – for all users;
- **ADMIN** – only for portal administrators. ||
|| **extranet**
[`unknown`](../../data-types.md) | Is the application available for extranet users (default is `N`). Supports the following values:
- **N** – not available for extranet users;
- **Y** – available for extranet users. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

IFRAME opens, but the context of the current chat is not passed to it (since navigation is an entity outside the chat context).

The application frame opens in the chat environment, mimicking the overall grid.

## Examples

{% list tabs %}

- PHP

    ```php
    CRest::call(
        'placement.bind',
        [
            'PLACEMENT' => 'IM_NAVIGATION',
            'HANDLER' => 'https://example.com/apps/immarket/handlers/navigation.php',
            'LANG_ALL' => [
                'de' => [
                    'TITLE' => 'Application for the left navigation menu',
                ],
            ],
            'OPTIONS' => [
                'iconName' => 'fa-check',
                'role' => 'USER',
                'extranet' => 'N',
            ]
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}