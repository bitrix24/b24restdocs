# Get Enumeration Items "Address Type" crm.enum.addresstype

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
crm.enum.addresstype()
```

Returns the enumeration items for "Address Type".

## Possible Values

```
{
    "result": [
        {
            "ID": 1,
            "NAME": "Actual Address"
        },
        {
            "ID": 4,
            "NAME": "Registration Address"
        },
        {
            "ID": 6,
            "NAME": "Legal Address"
        },
        {
            "ID": 9,
            "NAME": "Beneficiary Address"
        }
    ],
    "time": {
        "start": 1561544164.224608,
        "finish": 1561544164.245065,
        "duration": 0.020457029342651367,
        "processing": 0.008939027786254883,
        "date_start": "2019-06-26T13:16:04+02:00",
        "date_finish": "2019-06-26T13:16:04+02:00"
    }
}
```

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        "crm.enum.addresstype",
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

{% endlist %}


{% include [Examples Note](../../../../_includes/examples.md) %}