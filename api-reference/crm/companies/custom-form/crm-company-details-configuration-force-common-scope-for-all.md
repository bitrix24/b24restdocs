# Set a common card for all users crm.company.details.configuration.forceCommonScopeForAll

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.details.configuration.forceCommonScopeForAll` allows you to forcibly set a common company card for all users.

Without parameters

## Examples

{% list tabs %}

- JS
  
    ```js
    //--- 
    //Set a common company card for all users.
    BX24.callMethod(
        "crm.company.details.configuration.forceCommonScopeForAll",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    //---
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}