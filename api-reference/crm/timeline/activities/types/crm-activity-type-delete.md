# Delete Custom Activity Type crm.activity.type.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.type.delete` removes a subtype of activities.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **TYPE_ID**
[`unknown`](../../../../data-types.md) | Identifier of the provider's activity type. | ||
|#

## Example

```js
BX24.callMethod(
    'crm.activity.type.delete',
    {
        TYPE_ID: id
    },
    function(result)
    {
        if(result.error())
            alert("Error: " + result.error());
        else
        {
            alert("Success: " + result.data());
        }
    }
);
```

{% include [Example Note](../../../../../_includes/examples.md) %}