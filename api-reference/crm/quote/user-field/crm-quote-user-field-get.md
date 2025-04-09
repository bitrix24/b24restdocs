# Get Custom Fields of Estimates by ID crm.quote.userfield.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.userfield.get` returns the custom field of estimates by ID.

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the custom field. ||
|#

## Example

{% list tabs %}

- JS
  
    ```js
    var id = prompt("Enter ID");        
    BX24.callMethod(
        "crm.quote.userfield.get",
        { id: id },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}