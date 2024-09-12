# Removing a Block from My Blocks

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

{% note info "landing.landing.unFavoriteBlock" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.unFavoriteBlock` removes a block that was saved in "My Blocks".

## Parameters

#|
|| **Method** | **Description** ||
|| **blockId**
[`unknown`](../../../data-types.md) | The identifier of the block. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.landing.unFavoriteBlock',
    {
        blockId: 81827
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}