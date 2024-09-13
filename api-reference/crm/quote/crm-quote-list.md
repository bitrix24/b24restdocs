# Get a List of Estimates by Filter crm.quote.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameters need to be described here
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.list` returns a list of estimates based on the filter. It is an implementation of the list method for estimates.

See the description of [list methods](../../how-to-call-rest-api/list-methods-pecularities.md).

## Example

```javascript
BX24.callMethod(
    "crm.quote.list",
    {
        order: { "STATUS_ID": "ASC" },
        filter: { "=COMPANY_ID": 1 },
        select: [ "ID", "TITLE", "STATUS_ID", "OPPORTUNITY", "CURRENCY_ID" ]
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

{% include [Footnote on examples](../../../_includes/examples.md) %}