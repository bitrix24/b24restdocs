# Remove Card of Block landing.block.removecard

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
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

The method `landing.block.removecard` removes the card of the block. It returns *_true_* or an error.

## Parameters

#|
|| **Method** | **Description** | **Version** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **selector**
[`unknown`](../../../data-types.md) | [Card selector](../manifest.md#key-cards) taken from the manifest, with the card identifier added.
For example: '.landing-block-card@0'. The 0 at the end indicates that we are affecting the first card in order. | ||
|#

{% note warning %}

Please note that once you have removed the card, their counters have changed.

{% endnote %}

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.block.removecard',
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

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}