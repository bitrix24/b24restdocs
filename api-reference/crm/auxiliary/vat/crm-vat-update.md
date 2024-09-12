# Update Existing VAT Rate crm.vat.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- The requiredness and type of parameters are not specified
- No response in case of an error
- No response in case of success
- No examples in other languages

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.vat.update(id, fields)
```

This method updates an existing VAT rate.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the VAT rate. ||
|| **fields** | Set of fields - an array of the form `array("updating field"=>"value"[, ...])`, where "updating field" can take values from the method [crm.vat.fields](crm-vat-fields.md). ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Examples

```javascript
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.vat.update",
    {
        "id": id,
        "fields":
        {
            "ACTIVE": "N"
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
        {
            console.info(result.data());
        }
    }
);
```

{% include [Examples Note](../../../../_includes/examples.md) %}