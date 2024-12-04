# Get a list of included areas for the site landing.template.getSiteRef

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- response on success is missing
- response on error is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.template.getSiteRef` retrieves a list of included areas for the site. The keys of the returned array are the identifiers of the included areas, and the values are the identifiers of the pages.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Site identifier. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.template.getSiteRef',
        {
            id: 1
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

{% include [Footnote on examples](../../../_includes/examples.md) %}