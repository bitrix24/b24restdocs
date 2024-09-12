# Delete CRM Entity crm.status.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples (in other languages) are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.status.delete(id, params)
```

This method deletes a CRM entity.

#|
|| **Parameter** | **Description** ||
|| **id^*^** | Identifier of the CRM entity. ||
|| **params** | Set of parameters. FORCED - flag for forcibly deleting system entities. Default - N. If the entity being deleted is a system entity, it will not be deleted. If Y is passed, the entity will be deleted regardless. To delete a system entity, use the second example in the description. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

```javascript
var id = prompt("Enter the ID of the custom entity");
BX24.callMethod(
    "crm.status.delete",
    { id: id },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
);
```

```javascript
var id = prompt("Enter the ID of the custom or system entity");
BX24.callMethod(
    "crm.status.delete",
    { id: id, params:{ FORCED: "Y" } },
    function(result)
    {
     if(result.error())
            console.error(result.error());
     else
            console.info(result.data());
    }
);
```

{% include [Example Notes](../../../_includes/examples.md) %}