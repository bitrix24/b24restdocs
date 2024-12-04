# Add page landing.landing.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.landing.add` adds a page. It returns the `LID` of the created page or an error.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`unknown`](../../../data-types.md) | [Entity fields](../index.md) ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.landing.add',
        {
            fields: {
                TITLE: 'My first page!',
                CODE: 'firstpage',
                SITE_ID: 292,
                ADDITIONAL_FIELDS: {
                    THEME_CODE: 'wedding'
                }
            }
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% note warning %}

When creating a page, the theme code of the page (`THEME_CODE: 'wedding'`) is passed. This is necessary for the page to be in the corresponding [color scheme](../color-themes.md). If this is not done, the page will be in the default theme.

{% endnote %}