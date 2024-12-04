# Adding a Block to My Blocks

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response not provided
- error response not provided
- links to yet-to-be-created pages not specified (1 link)

{% endnote %}

{% endif %}

{% note info "landing.landing.favoriteBlock" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.favoriteBlock` saves the existing block on the page to "My Blocks". It returns the identifier of the newly saved block.

{% note info %}

This method may be useful when removing a block from saved items.

{% endnote %}

## Parameters

#|
|| **Method** | **Description** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier. ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier. ||
|| **meta**
[`unknown`](../../../data-types.md) | Information object for saving the block. Contains fields:
- **name** – block name;
- **section** – array of [categories](../../block/manifest.md) to save the block to;
- **preview** – block image. ||
|#

## Examples

{% list tabs %}

- JS

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

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}