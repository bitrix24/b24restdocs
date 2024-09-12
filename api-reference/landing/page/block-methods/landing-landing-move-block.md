# Moving a Block from One Page to Another

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

{% note info "landing.landing.moveblock" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.moveblock` transfers a block from one page to another. It returns *true* or an error.

## Parameters

#|
|| **Method** | **Description** ||
|| **lid**
[`unknown`](../../../data-types.md) | The identifier of the page to which the block should be copied. ||
|| **block**
[`unknown`](../../../data-types.md) | The identifier of the block, which may be on another page (the operation only makes sense in this case). ||
|| **params**
[`unknown`](../../../data-types.md) | An array of parameters, currently supporting one key - AFTER_ID - after which block to insert the new one. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.landing.moveblock',
    {
        lid: 349,
        block: 6428
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