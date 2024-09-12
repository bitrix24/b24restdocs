# Change the anchor symbol code landing.block.changeAnchor

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.changeAnchor` changes the anchor symbol code. By default, the anchor looks like this: `#block12345`, where 12345 is the block identifier.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier. | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier. | ||
|| **data**
[`unknown`](../../../data-types.md) | Anchor symbol code. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.block.changeAnchor',
    {
        lid: 3496,
        block: 29356,
        data: 'about'
    },
    function (result)
    {
        if (result.error())
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