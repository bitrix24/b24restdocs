# Delete VAT Rate crm.vatdelete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- type and required parameter specifications are missing
- no response for error and success cases
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
crm.vat.delete(id)
```

Deletes a VAT rate.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the VAT rate. ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Examples

```javascript
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.vat.delete",
    { "id": id },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
);
```

{% include [Examples Note](../../../../_includes/examples.md) %}