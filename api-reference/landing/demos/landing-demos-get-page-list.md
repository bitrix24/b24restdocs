# Get a List of Templates for Creating Pages landing.demos.getPageList

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameters or fields
- required parameters not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.demos.getPageList` retrieves a list of available templates for creating pages, both partner and system templates.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **type**
[`unknown`](../../data-types.md) | Template type (page: regular websites, store: stores). ||
|#

## Example

```js
BX24.callMethod(
    'landing.demos.getPageList',
    {
        type: 'page'
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

{% include [Footnote on examples](../../../_includes/examples.md) %}