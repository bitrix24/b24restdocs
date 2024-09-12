# Restore Site from Trash landing.site.markUnDelete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.markUnDelete` marks the site as not deleted.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **ID**
[`unknown`](../../data-types.md) | Page identifier | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.markUnDelete',
    {
        id: 1688
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

{% include [Note on examples](../../../_includes/examples.md) %}