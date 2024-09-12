# Update System CRM Activity

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- revisions needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

Since CRM version 22.1350.0, this method is deprecated. Use the universal activity methods:
- [{#T}](./todo-update/crm-activity-todo-update-deadline.md)
- [{#T}](./todo-update/crm-activity-todo-update-description.md).

{% endnote %}

The method `crm.activity.update` updates an existing activity.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the activity. ||
|| **fields**
[`array`](../../../data-types.md) | Set of fields - an array in the form array("field_to_update"=>"value"[, ...]), where "field_to_update" can take values returned by the method [crm.activity.fields](./crm-activity-fields.md). 

{% note info %}

To find out the required format for the fields, execute the method [crm.activity.fields](./crm-activity-fields.md) and check the format of the returned values for those fields.

{% endnote %}

 ||
|#

## Example

```js
var d = new Date();
d.setSeconds(0);
var dateStr = d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+00:00';
var paddatepart = function(part)
{
    return part >= 10 ? part.toString() : '0' + part.toString();
}
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.activity.update",
    {
        id: id,
        fields:
        {
            "START_TIME": dateStr,
            "END_TIME": dateStr,
            COMPLETED: 'Y'
        }
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

{% include [Example Note](../../../../_includes/examples.md) %}