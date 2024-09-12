# Adding a Block to My Blocks

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
- links to pages not yet created are not provided (1 link)

{% endnote %}

{% endif %}

{% note info "landing.landing.favoriteBlock" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.favoriteBlock` saves the existing block on the page to "My Blocks". It returns the identifier of the newly saved block.

{% note info %}

This method can be useful when deleting a block from saved items.

{% endnote %}

## Parameters

#|
|| **Method** | **Description** ||
|| **lid**
[`unknown`](../../../data-types.md) | Identifier of the page. ||
|| **block**
[`unknown`](../../../data-types.md) | Identifier of the block. ||
|| **meta**
[`unknown`](../../../data-types.md) | Information object for saving the block. Contains fields:
- **name** – name of the block;
- **section** – array of [categories](../../block/manifest.md) to save the block to;
- **preview** – image of the block. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.landing.favoriteBlock',
    {
        lid: 11262,
        block: 81827,
        meta: {
            name: 'My Block',
            section: ['text', 'text_image'],
            preview: 'https://mycdn.com/pic/1.jpg'
        }
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