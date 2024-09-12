# Get a List of Companies by Filter crm.company.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- missing examples
- missing success response
- missing error response

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.list` returns a list of companies based on a filter. It is an implementation of the list method for companies.

When selecting, use masks:
- "*" - to select all fields (excluding custom and multiple fields)
- "UF_*" - to select all custom fields (excluding multiple fields)

There is no mask for selecting multiple fields. To select multiple fields, specify the required ones in the selection list ("PHONE", "EMAIL", etc.).

## Parameters

See the description of [list methods](../../how-to-call-rest-api/list-methods-pecularities.md).

## Examples

**Searching for companies by industry and type**

```js
BX24.callMethod(
    "crm.company.list",
    {
        order: { "DATE_CREATE": "ASC" },
        filter: { "INDUSTRY": "MANUFACTURING", "COMPANY_TYPE": "CUSTOMER" },
        select: [ "ID", "TITLE", "CURRENCY_ID", "REVENUE" ]
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

**Searching for a company by phone number**

```js
BX24.callMethod(
    "crm.company.list",
    {
        filter: { "PHONE": "555888" },
        select: [ "ID", "TITLE" ]
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