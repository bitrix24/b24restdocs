# Get a List of VAT Rates by Filter crm.vat.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the requiredness of parameters is not specified
- there is no response in case of error and success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Returns a list of VAT rates based on the filter. This is an implementation of the list method for VAT rates.

See the description of [list methods](../../../../api-reference/how-to-call-rest-api/list-methods-pecularities.md).

## Examples

```javascript
BX24.callMethod(
    "crm.vat.list",
    {
        "order": { "ID": "ASC" },
        "filter": { "ACTIVE": "Y" },
        "select": [ "ID", "NAME", "RATE" ]
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
        {
            console.dir(result.data());
            if(result.more())
                result.next();
        }
    }
);
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}