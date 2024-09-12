# Get Block Content from Repository landing.block.getContentFromRepository

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

The method `landing.block.getContentFromRepository` retrieves the block content from the repository "as is" before adding the block to any page.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **code**
[`unknown`](../../../data-types.md) | block code | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.block.getContentFromRepository',
    {
        code: '28.6.team_4_cols'
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
);
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}