# Get a List of Custom Company Fields by Filter crm.company.userfield.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.userfield.list` returns a list of custom company fields based on the filter.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **order**
[`array`](../../../data-types.md) | Sorting fields. ||
|| **filter**
[`array`](../../../data-types.md) | Filter fields. ||
|| **START** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Example

```js
BX24.callMethod(
    "crm.company.userfield.list",
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

{% include [Footnote about examples](../../../../_includes/examples.md) %}