# Remove from menu landing.site.unbindingFromMenu

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

The method `landing.site.unbindingFromMenu` removes the Knowledge Base binding from the menu. Read access to the Knowledge Base is required.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the Knowledge Base. | ||
|| **menuCode**
[`unknown`](../../../data-types.md) | Symbolic code of the menu, as defined above. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.unbindingFromMenu',
    {
        id: 31,
        menuCode: 'crm_switcher:deal'
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

{% include [Footnote about examples](../../../../_includes/examples.md) %}