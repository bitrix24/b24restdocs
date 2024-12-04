# Get the list of services ai.engine.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`ai_admin`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method without parameters returns a list of registered [engines](./ai-engine-register.md) for the current partner. The method is called without parameters.

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'ai.engine.list',
        {
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

{% include [Footnote about examples](../../_includes/examples.md) %}