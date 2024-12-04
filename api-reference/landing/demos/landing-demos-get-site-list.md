# Get a list of templates for creating sites landing.demos.getSiteList

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

The method `landing.demos.getSiteList` retrieves a list of available templates for creating sites, both partner and system templates.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **type**
[`unknown`](../../data-types.md) | Template type (page: regular sites, store: stores). ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.demos.getSiteList',
        {
            type: 'page'
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

{% include [Footnote about examples](../../../_includes/examples.md) %}