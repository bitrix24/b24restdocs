# Get the list of mail services mailservice.list

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response examples

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `mailservice.list` returns a list of all mail services.

## Parameters

No parameters.

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "mailservice.list",
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