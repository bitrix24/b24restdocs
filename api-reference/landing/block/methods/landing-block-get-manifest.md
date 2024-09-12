# Get Manifest of Block landing.block.getmanifest

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
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

The method `landing.block.getmanifest` retrieves the manifest of a specific block that is already placed on the page. It returns the block's manifest or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **params**
[`unknown`](../../../data-types.md) | Parameters: **edit_mode** - editing mode (1) or not (0 - default) | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.block.getmanifest',
    {
        lid: 313,
        block: 6102,
        params: {
            edit_mode: 0
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