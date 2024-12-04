# Restore site from trash landing.site.markUnDelete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

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

The method `landing.site.markUnDelete` marks the site as not deleted.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **ID**
[`unknown`](../../data-types.md) | Page identifier | ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.site.markUnDelete',
        {
            id: 1688
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