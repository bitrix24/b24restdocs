# Execute Bulk Update of Cards in landing.block.updateCards

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.updateCards` is used for bulk updating cards in a block. It will return _true_ on success, or an error.

{% note warning %}

1. The method will completely remove the current cards in the block.
2. This method is specific and is recommended for use only if your tasks cannot be solved by [landing.block.updatenodes](./landing-block-update-nodes.md).

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier. | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier. | ||
|| **data**
[`unknown`](../../../data-types.md) | Array for modification. For clarification, see the example. Optionally, you can pass [card presets](../extended-description.md).
Note that selectors are formed using a similar method to that in _landing.block.updatenodes_. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.block.updateCards',
    {
        lid: 2856,
        block: 25458,
        data: {
            // we are affecting this card selector
            // (other selectors can be passed simultaneously)
            '.landing-block-card': {
                // only this number of cards will remain, 
                // with only the specified nodes being changed;
                // the first card will be used for cloning
                'values': [
                    {
                        '.landing-block-node-title': 'New title 0'
                    },
                    {
                        '.landing-block-node-title': 'New title 1'
                    },
                    {
                        '.landing-block-node-title': 'New title 2'
                    }
                ],
                // optionally, card presets can be applied (key - card number, starting from 0)
                'presets': {
                    '1': 'preset_h2'
                }
            }
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