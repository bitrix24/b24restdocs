# Add VAT Rate crm.vat.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- type and required parameter specifications are missing
- no success response
- no error response
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
|| **fields** | A set of fields - an array in the form `array("field"=>"value"[, ...])`, containing the values of the VAT rate. ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```javascript
    var current = new Date();
    var date2str = function(d)
    {
        return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours())
            + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+01:00';
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

{% endlist %}

{% include [Note on examples](../../../../_includes/examples.md) %}