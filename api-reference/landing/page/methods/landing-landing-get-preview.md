# Getting the Path to the Page Preview

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

{% note info "landing.landing.getpreview" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.getpreview` returns the path to the page preview or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | page identifier. ||
|#

## Example

```js
BX24.callMethod(
    'landing.landing.getpreview',
    {
        lid: 351
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