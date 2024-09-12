# Get the block manifest from the repository landing.block.getmanifestfile

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.getmanifestfile` retrieves the block manifest from the repository. It will return the block manifest or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **code**
[`unknown`](../../../data-types.md) | block code | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.block.getmanifestfile',
    {
        code: '01.big_with_text'
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

{% include [Example notes](../../../../_includes/examples.md) %}