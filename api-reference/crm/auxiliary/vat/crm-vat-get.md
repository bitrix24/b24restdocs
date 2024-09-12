# Get VAT Rate by ID crm.vat.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- The requirement for parameters is not specified
- There is no response in case of error and success
- No examples in other languages
  
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
|| **id** | The identifier of the VAT rate. ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

## Examples

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

{% include [Note on examples](../../../../_includes/examples.md) %}