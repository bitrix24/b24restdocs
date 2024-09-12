# Get a List of Blocks from the Repository landing.block.getrepository

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.getrepository` returns a list of blocks from the repository or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **section**
[`unknown`](../../../data-types.md) | [Section code](#block) of the repository (list provided above) | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.block.getrepository',
    {
        section: 'about'
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