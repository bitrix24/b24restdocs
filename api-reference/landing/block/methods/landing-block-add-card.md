# Add Card with Modified Content landing.block.addcard

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.addcard` fully replicates the functionality of [landing.block.clonecard](./landing-block-clone-card.md) but allows you to insert a card with modified content right away.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **selector**
[`unknown`](../../../data-types.md) | [Card selector](../manifest.md#key-cards) taken from the manifest, with the card identifier added.
For example: `.landing-block-card@0`. The 0 at the end indicates that we are affecting the first card in order. | ||
|| **content**
[`unknown`](../../../data-types.md) | Content of the new card. | ||
|#

{% note warning %}

Please note that once you clone a card, their counters change.

{% endnote %}

## Examples

```js
BX24.callMethod(
    'landing.block.addCard',
    {
        lid: 634,
        block: 12079,
        selector: '.landing-block-node-menu-list-item@0',
        content: '<li class="landing-block-node-menu-list-item nav-item g-mx-30--lg g-mb-7 g-mb-0--lg">' + '<a href="#about" class="landing-block-node-menu-list-item-link nav-link g-color-white p-0">New card item</a>' + '</li>'
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