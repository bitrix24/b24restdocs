# Bind to Social Network Group landing.site.bindingToGroup

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.bindingToGroup` binds a specific Knowledge Base to a group. The user must be a member of the specified group, and the group must not have a Knowledge Base bound to it.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the Knowledge Base. | ||
|| **groupId**
[`unknown`](../../../data-types.md) | Identifier of the group. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.bindingToGroup',
    {
        id: 32,
        groupId: 174
    },
    function(result)
    {
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            console.info(result.data());
        }
    }
);
```

{% include [Example Note](../../../../_includes/examples.md) %}