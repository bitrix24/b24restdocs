# Unbind from Social Network Group landing.site.unbindingFromGroup

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.unbindingFromGroup` unbinds a specific Knowledge Base from a group. The user must be a member of the specified group.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the Knowledge Base. | ||
|| **groupId**
[`unknown`](../../../data-types.md) | Identifier of the group. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.unbindingFromGroup',
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

{% include [Example Notes](../../../../_includes/examples.md) %}