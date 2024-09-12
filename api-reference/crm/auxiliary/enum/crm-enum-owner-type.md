# Get Enumeration Elements "Owner Type" crm.enum.ownertype

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing response in case of error
- missing response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
crm.enum.ownertype()
```

This method returns the identifiers of CRM entity types and SPAs.

## Possible Values

```json
{
"result": [
    {
     "ID": 1,
     "NAME": "Lead",
     "SYMBOL_CODE": "LEAD",
     "SYMBOL_CODE_SHORT": "L"
},
{
     "ID": 2,
     "NAME": "Deal",
     "SYMBOL_CODE": "DEAL",
     "SYMBOL_CODE_SHORT": "D"
},
{
     "ID": 3,
     "NAME": "Contact",
     "SYMBOL_CODE": "CONTACT",
     "SYMBOL_CODE_SHORT": "C"
},
{
     "ID": 4,
     "NAME": "Company",
     "SYMBOL_CODE": "COMPANY",
     "SYMBOL_CODE_SHORT": "CO"
},
{
     "ID": 5,
     "NAME": "Invoice (old version)",
     "SYMBOL_CODE": "INVOICE",
     "SYMBOL_CODE_SHORT": "I"
},
{
     "ID": 31,
     "NAME": "Invoice",
     "SYMBOL_CODE": "SMART_INVOICE",
     "SYMBOL_CODE_SHORT": "SI"
},
{
     "ID": 7,
     "NAME": "Estimate",
     "SYMBOL_CODE": "QUOTE",
     "SYMBOL_CODE_SHORT": "Q"
},
{
     "ID": 8,
     "NAME": "Requisites",
     "SYMBOL_CODE": "REQUISITE",
     "SYMBOL_CODE_SHORT": "RQ"
},
{
     "ID": 130,
     "NAME": "All Inclusive",
     "SYMBOL_CODE": "DYNAMIC_130",
     "SYMBOL_CODE_SHORT": "T82"
}
],
"time": {
"start": 1652769631.135543,
"finish": 1652769631.151046,
"duration": 0.0155029296875,
"processing": 0.0014200210571289062,
"date_start": "2022-05-17T09:40:31+03:00",
"date_finish": "2022-05-17T09:40:31+03:00",
"operating": 0
}
}
```

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    "crm.enum.ownertype",
    {},
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);     
```

{% include [Example Note](../../../../_includes/examples.md) %}