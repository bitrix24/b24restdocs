# Delete SMS Provider or Message Provider messageservice.sender.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- success response is missing
- error response is missing
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

{% include [Parameter Note](../../_includes/required.md) %}

## Example

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

{% include [Examples Note](../../_includes/examples.md) %}