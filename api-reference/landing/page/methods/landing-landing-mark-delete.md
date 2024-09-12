# Marking a Page as Deleted

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% note info "landing.landing.markDelete" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.markDelete` marks a page as deleted.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **$lid**
[`unknown`](../../../data-types.md) | Page identifier. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.landing.markDelete',
    {
        lid: 1688
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