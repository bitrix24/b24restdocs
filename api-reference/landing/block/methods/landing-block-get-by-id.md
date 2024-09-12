# Get Block by ID landing.block.getbyid

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.getbyid` retrieves a block by its identifier. It returns the block or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **params**
[`unknown`](../../../data-types.md) | Parameters: **edit_mode** - Editing mode (1) or not (0 - default), will return a different set of blocks. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.block.getbyid',
    {
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