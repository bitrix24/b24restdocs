# Get a List of Included Areas for the landing.template.getLandingRef Page

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.template.getLandingRef` retrieves a list of included areas for the page. The keys of the returned array are the identifiers of the included areas, while the values are the identifiers of the pages.

## Parameters

#|
|| **Method** | **Description** ||
|| **ID**
[`unknown`](../../data-types.md) | Page identifier ||
|#

## Examples

```js
BX24.callMethod(
    'landing.template.getLandingRef',
    {
        id: 557
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

{% include [Footnote on examples](../../../_includes/examples.md) %}