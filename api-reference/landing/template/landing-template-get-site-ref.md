# Get a list of included areas for the site landing.template.getSiteRef

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.template.getSiteRef` retrieves a list of included areas for the site. The keys of the returned array are the identifiers of the included areas, and the values are the page identifiers.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Site identifier. ||
|#

## Example

```js
BX24.callMethod(
    'landing.template.getSiteRef',
    {
        id: 1
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