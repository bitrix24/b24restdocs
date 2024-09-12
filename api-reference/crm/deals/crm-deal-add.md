# Create a New Deal crm.deal.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

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

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.add` creates a new deal.

#|
|| **Parameter** | **Description** ||
|| **fields** | A set of fields – an array of the form `array("field"=>"value"[, ...])`, containing the values of the deal fields. 

{% note info "" %} 

To find out the required format of the fields, execute the method [crm.deal.fields](./crm-deal-fields.md) and check the format of the received values for these fields. 

{% endnote %} ||
|| **params** | A set of parameters. `REGISTER_SONET_EVENT` – to register the event of adding a deal in the live feed. A notification will also be sent to the person responsible for the deal. ||
|#

## Example

```js
var current = new Date();
var nextMonth = new Date();
nextMonth.setMonth(current.getMonth() + 1);
var date2str = function(d)
{
     return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+03:00';
};
var paddatepart = function(part)
{
     return part >= 10 ? part.toString() : '0' + part.toString();
};
    
BX24.callMethod(
    "crm.deal.add",
    {
        fields:
        {
            "TITLE": "Planned Sale",
            "TYPE_ID": "GOODS",
            "STAGE_ID": "NEW",
            "COMPANY_ID": 3,
            "CONTACT_ID": 3,
            "OPENED": "Y",
            "ASSIGNED_BY_ID": 1,
            "PROBABILITY": 30,
            "CURRENCY_ID": "USD",
            "OPPORTUNITY": 5000,
            "CATEGORY_ID": 5,
            "BEGINDATE": date2str(current),
            "CLOSEDATE": date2str(nextMonth)                    
        },
        params: { "REGISTER_SONET_EVENT": "Y" }    
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info("Deal created with ID " + result.data());
    }
);
```

{% include [Note on Examples](../../../_includes/examples.md) %}


{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-add.md)

{% endnote %}