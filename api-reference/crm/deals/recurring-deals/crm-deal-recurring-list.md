# Get a list of recurring deal template settings crm.deal.recurring.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.recurring.list` returns a list of recurring deal template settings based on the filter.

When querying, use the mask "*" to select all fields (excluding custom and multiple fields).

See the description of [list methods](../../../how-to-call-rest-api/list-methods-pecularities.md).

## Example

```js
BX24.callMethod(
    "crm.deal.recurring.list",
    {
        order: { "DEAL_ID": "ASC" },
        filter: { ">COUNTER_REPEAT": 5 },
        select: [ "ID", "DEAL_ID ", "NEXT_EXECUTION", "LAST_EXECUTION", "CATEGORY_ID", "IS_LIMIT" ]
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

{% include [Footnote about examples](../../../../_includes/examples.md) %}