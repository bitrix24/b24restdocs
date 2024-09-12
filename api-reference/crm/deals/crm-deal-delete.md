# Delete deal crm.deal.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples (in other languages) are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.delete` removes a deal and all associated objects.

Deleting a deal will result in the removal of all related objects, such as tasks, history, Timeline activities, and others. Objects are deleted if they are not linked to other entities or items. If the objects are linked to other entities, only the link to the deleted deal will be removed.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Identifier of the deal. ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Example

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.deal.delete",
    { id: id },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info("Deal with ID " + id + " has been successfully deleted.");
    }
);
```

{% include [Example notes](../../../_includes/examples.md) %}


{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-delete.md)

{% endnote %}