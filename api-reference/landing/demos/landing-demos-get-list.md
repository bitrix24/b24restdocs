# Get a list of available partner templates landing.demos.getList

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

The method `landing.demos.getList` retrieves a list of available partner templates for the current application.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **params**
[`unknown`](../../data-types.md) | An optional array with optional keys:
- select
- filter
- order
- group
which contain values from the main fields table of the entity. The table is provided below. ||
|#

## Entity Fields

#|
|| **Field** | **Description** ||
|| **ID**
[`unknown`](../../data-types.md) | Record identifier. ||
|| **XML_ID**
[`unknown`](../../data-types.md) | Unique record code. ||
|| **APP_CODE**
[`unknown`](../../data-types.md) | Code of the current application. ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Activity status (Y / N). ||
|| **TITLE**
[`unknown`](../../data-types.md) | Title. ||
|| **DESCRIPTION**
[`unknown`](../../data-types.md) | Description. ||
|| **PREVIEW_URL**
[`unknown`](../../data-types.md) | Preview URL. ||
|| **TYPE**
[`unknown`](../../data-types.md) | Type of the created site (STORE, PAGE). ||
|| **TPL_TYPE**
[`unknown`](../../data-types.md) | Placed in the site/store (S) or page (P) creation wizard. ||
|| **MANIFEST**
[`unknown`](../../data-types.md) | Manifest. ||
|| **SHOW_IN_LIST**
[`unknown`](../../data-types.md) | Show in the list of templates. ||
|| **PREVIEW / PREVIEW2X / PREVIEW3X**
[`unknown`](../../data-types.md) | Various sizes of previews. ||
|| **CREATED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who created the record. ||
|| **MODIFIED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who modified the record. ||
|| **DATE_CREATE**
[`unknown`](../../data-types.md) | Creation date. ||
|| **DATE_MODIFY**
[`unknown`](../../data-types.md) | Modification date. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.demos.getList',
        {
            params: {
                select: [
                    'ID', 'TITLE', 'MANIFEST'
                ],
                filter: {
                    '>ID': '1'
                }
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}