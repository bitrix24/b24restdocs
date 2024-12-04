# Remove embedding locations landing.repo.unbind

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing parameters or fields
- parameter types not specified
- required parameters not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The removal is performed by the module's own method `landing.repo.unbind`, which simply takes the embedding location code as an argument. All embedding locations with this code will be removed. If the application has registered multiple locations with different paths, a specific one can be removed by passing the embedding location address.

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.repo.unbind',
        {
            code: 'LANDING_SETTINGS',
    //        handler: 'https://site.com/rt/placement.php?version=3'
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