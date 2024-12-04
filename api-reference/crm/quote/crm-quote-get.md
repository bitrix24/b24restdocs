# Get an estimate by ID crm.quote.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.get` returns an estimate by its ID.

#| 
||  **Parameter** / **Type**| **Description** || 
|| **id** 
[`unknown`](../../data-types.md) | The identifier of the estimate. || 
|#

## Example

{% list tabs %}

- JS

    ```javascript
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.quote.get",
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