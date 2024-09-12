# Remove Block landing.repo.unregister

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.repo.unregister` removes a block. It returns *true* upon successful deletion or *false* if the block has already been deleted or did not exist.

## Parameters

#|
|| **Method** | **Description** ||
|| **code**
[`unknown`](../../data-types.md) | Unique code of the block to be deleted. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.repo.unregister',
    {code: 'myblockx'},
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
);
```

{% include [Footnote on examples](../../../_includes/examples.md) %}