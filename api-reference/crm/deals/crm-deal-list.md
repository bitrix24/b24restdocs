# Get a List of Deals crm.deal.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.list` returns a list of deals based on a filter. It is an implementation of the list method for deals.

When selecting, use the masks:
- "*" - to select all fields (excluding custom and multiple)
- "UF_*" - to select all custom fields (excluding multiple)

See the description of [list methods](../../how-to-call-rest-api/list-methods-pecularities.md).

## Examples

The example outputs data to the console. If you need to output data differently, implement your own data handling for the responses returned by **result.data()** and **result.error()**.

```js
BX24.callMethod(
    "crm.deal.list",
    {
        order: { "STAGE_ID": "ASC" },
        filter: { ">PROBABILITY": 50 },
        select: [ "ID", "TITLE", "STAGE_ID", "PROBABILITY", "OPPORTUNITY", "CURRENCY_ID" ]
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

Searching across multiple stages at once:

```js
filter: {"=STAGE_ID": ["C5:NEW", "C5:3", "C5:PREPARATION", "C5:PREPAYMENT_INVOICE", "C5:2", "C5:4"] }
```

{% include [Note on Examples](../../../_includes/examples.md) %}


{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-list.md)

{% endnote %}