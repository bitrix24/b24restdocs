# Get a list of included areas for the page landing.template.getLandingRef

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.template.getLandingRef` retrieves a list of included areas for the page. The keys of the returned array are the identifiers of the included areas, and the values are the identifiers of the pages.

## Parameters

#|
|| **Method** | **Description** ||
|| **ID**
[`unknown`](../../data-types.md) | Page identifier ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.template.getLandingRef',
        {
            id: 557
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



{% include [Footnote about examples](../../../_includes/examples.md) %}