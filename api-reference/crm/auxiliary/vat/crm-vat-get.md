# Get VAT Rate by ID crm.vat.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error and success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
crm.vat.get(id)
```

Returns the VAT rate by ID.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the VAT rate. ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS
  
    ```javascript
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.vat.get",
        { "id": id },
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


{% include [Note on examples](../../../../_includes/examples.md) %}