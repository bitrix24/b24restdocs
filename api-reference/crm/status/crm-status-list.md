# Get a List of Directory Items by Filter crm.status.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.status.list()
```

The method returns a list of directory items based on the filter. It is an implementation of the list method for directory items. Please note that in this implementation, the "select" and "navigation" parameters are not supported.

## Parameters

See the description of [list methods](../../../api-reference/how-to-call-rest-api/list-methods-pecularities.md).

## Examples

```javascript
BX24.callMethod(
    "crm.status.list",
    {
        order: { "SORT": "ASC" },
        filter: { "ENTITY_ID": "STATUS" }
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