# Add VAT Rate crm.vat.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- type and requiredness of parameters are not specified
- no response in case of success
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
crm.vat.add(fields)
```

This method creates a new VAT rate.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields** | A set of fields - an array of the form `array("field"=>"value"[, ...])`, containing the values of the VAT rate. ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Examples

```javascript
var current = new Date();
var date2str = function(d)
{
    return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours())
        + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+03:00';
};
var paddatepart = function(part)
{
    return part >= 10 ? part.toString() : '0' + part.toString();
};
BX24.callMethod(
    "crm.vat.add",
    {
        "fields":
        {
            "TIMESTAMP_X": date2str(current),
            "ACTIVE": "Y",
            "C_SORT": 110,
            "NAME": "VAT 18%",
            "RATE": 18.00
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info("A new VAT rate has been created with ID " + result.data());
    }
);
```

{% include [Examples Note](../../../../_includes/examples.md) %}