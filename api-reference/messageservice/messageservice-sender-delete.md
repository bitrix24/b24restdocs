# Delete SMS provider or messageservice.sender.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no success response is provided
- no error response is provided
- No examples in other languages

{% endnote %}

{% endif %}

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a previously registered message provider. You cannot delete a provider registered by another application or webhook.

#|
|| **Parameter** | **Description** ||
|| **CODE** | Provider identifier. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    function uninstallProvider(provider)
    {
        BX24.callMethod(
            'messageservice.sender.delete',
            {
                'CODE': provider
            },
            function(result)
            {
                if(result.error())
                    alert('Error: ' + result.error());
                else
                    alert("Success: " + result.data());
            }
        );
    }
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}