# Clone the card of landing.block.clonecard

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits are needed to meet the writing standard
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

The method `landing.block.clonecard` clones a block card. It returns _true_ or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **selector**
[`unknown`](../../../data-types.md) | [Card selector](../manifest.md#key-cards) taken from the manifest, with the added card identifier.
For example: '.landing-block-card@0'. The 0 at the end indicates that we are affecting the first card in order. If the @ sign and the number following it are absent, the insertion will be made at the beginning of the parent container of the cards. | ||
|#

{% note warning %}

Please note that once you have cloned a card, their counters have changed.

{% endnote %}

## Examples

```js
BX24.callMethod(
    'landing.block.cloneCard',
    {
        lid: 311,
        block: 6057,
        selector: '.landing-block-card@0'
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

## See also

[landing.block.addcard](./landing-block-add-card.md) - This method fully replicates the functionality of `landing.block.clonecard` but allows you to insert a card immediately with modified content.