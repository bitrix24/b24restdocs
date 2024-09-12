# Update Existing Settings for the Recurring Deal Template crm.deal.recurring.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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

The method `crm.deal.recurring.update` updates the existing settings for the recurring deal template.

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the recurring deal template settings. ||
|| **fields** | Set of fields – an array of the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values returned by the method [crm.deal.recurring.fields](./crm-deal-recurring-fields.md). 

{% note info %}

To find out the required format of the fields, execute the method [crm.deal.recurring.fields](./crm-deal-recurring-fields.md) and check the format of the returned values for those fields. 

{% endnote %}
||
|#

## Example

```js
var current = new Date();
var nextYear = new Date();
nextYear.setYear(current.getFullYear() + 1);
var date2str = function(d)
{
    return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+03:00';
};
var paddatepart = function(part)
{
    return part >= 10 ? part.toString() : '0' + part.toString();
};
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.deal.recurring.update",
    {
        id: id,
        fields:
        {
            "CATEGORY_ID": "2",
            "START_DATE": date2str(nextYear),
            "PARAMS": {
                "MODE": "single",
                "SINGLE_BEFORE_START_DATE_TYPE": "day",
                "SINGLE_BEFORE_START_DATE_VALUE": 5,
                "OFFSET_BEGINDATE_TYPE": "day",
                "OFFSET_BEGINDATE_VALUE": 1,
                "OFFSET_CLOSEDATE_TYPE": "month",
                "OFFSET_CLOSEDATE_VALUE": 2,
            }
        },
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
        {
            console.info(result.data());
        }
    }
);
```

{% include [Examples Note](../../../../_includes/examples.md) %}