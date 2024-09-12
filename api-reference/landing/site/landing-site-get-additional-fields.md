# Get Additional Fields of the Site landing.site.getadditionalfields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

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

The method `landing.site.getadditionalfields` retrieves [additional fields](./additional-fields.md) of the site. It returns the additional fields of the site or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **id**
[`unknown`](../../data-types.md) | site identifier | ||
|#

## Example

```js
BX24.callMethod(
    'landing.site.getadditionalfields',
    {
        id: 205
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