# Get Content of Block landing.block.getcontent

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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

The method `landing.block.getcontent` retrieves the content of a block. It returns an array of block content - html, style files, and JS. Or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **editMode**
[`unknown`](../../../data-types.md) | Editing mode (1) or not (0), will return a different set of blocks | ||
|| **params**
[`unknown`](../../../data-types.md) | Array of additional parameters. Currently supports one key – **wrapper_show** – whether to show the enclosing system div (0, 1). Default - show. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.block.getContent',
    {
        lid: 4858,
        block: 39556,
        editMode: 1,
        params: {
            wrapper_show: 0
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

{% include [Note on examples](../../../../_includes/examples.md) %}