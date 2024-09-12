# Create a New Deal from the Template crm.deal.recurring.expose

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter type is not specified
- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.recurring.expose` creates a new deal from a template.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Identifier of the recurring deal template setting. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Example

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.deal.recurring.expose",
    {
        id: id,
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

{% include [Example Notes](../../../../_includes/examples.md) %}