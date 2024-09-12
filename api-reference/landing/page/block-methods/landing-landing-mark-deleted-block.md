# Marking a Block as Deleted

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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

{% note info "landing.landing.markdeletedblock" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.markdeletedblock` marks a block as deleted but does not physically remove it.

## Parameters

#|
|| **Method** | **Description** ||
|| **lid**
[`unknown`](../../../data-types.md) | Identifier of the page. ||
|| **block**
[`unknown`](../../../data-types.md) | Identifier of the block. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.landing.markdeletedblock',
    {
        lid: 627,
        block: 11923
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