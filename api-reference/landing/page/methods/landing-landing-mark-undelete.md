# Marking a Page as Not Deleted

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

{% note info "landing.landing.markUnDelete" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.markUnDelete` marks a page as not deleted.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **$lid**
[`unknown`](../../../data-types.md) | Page identifier. ||
|#

## Example

```js
BX24.callMethod(
    'landing.landing.markUnDelete',
    {
        lid: 1688
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