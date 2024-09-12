# Delete service ai.engine.unregister

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- response in case of error is absent
- links to pages that have not yet been created are not provided.

{% endnote %}

{% endif %}

> Scope: [`ai_admin`](../scopes/permissions.md)
>
> Who can execute the method: administrator

Method for deleting [engine](./ai-engine-register.md).

#|
|| **Parameters** | **Description** ||
|| **code**
[`unknown`](../data-types.md) | Engine code ||
|#

## Examples

```js
BX24.callMethod(
    'ai.engine.unregister',
    {
        code: 'smith_gpt',
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

{% include [Note on examples](../../_includes/examples.md) %}

## Response in case of success

On success, returns `true`.