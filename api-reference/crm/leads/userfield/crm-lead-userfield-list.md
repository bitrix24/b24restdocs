# Get a List of Fields crm.lead.userfield.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples (in other languages) are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.userfield.list` returns a list of custom fields for leads based on the filter.

#|
|| **Parameter** | **Description** ||
|| **order** | Sorting fields. ||
|| **filter** | Filtering fields. ||
|#

## Example

```js
BX24.callMethod(
    "crm.lead.userfield.list",
    {
        order: { "SORT": "ASC" },
        filter: { "MANDATORY": "N" }
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