# Get a List of Pages landing.landing.getList

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.landing.getList` retrieves a list of pages.

{% note warning %}

Pages marked as deleted do not appear in the selections. To explicitly retrieve them, you need to specify the key `DELETED` with the value Y or N in the filter.

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **params**
[`unknown`](../../../data-types.md) | An optional array with optional keys: **select**, **filter**, **order**, **group**, which contain values from the [main fields](../index.md) of the entity.
Additionally, you can pass flags `get_preview = 1` (to return page previews), `get_urls = 1` (to return public addresses of pages), `check_area` (to return the flag IS_AREA indicating whether the page is an included area). ||
|#

## Examples

```js
BX24.callMethod(
    'landing.landing.getList',
    {
        params: {
            select: [
                'ID', 'TITLE'
            ],
            filter: {
                TITLE: '%services%',
                SITE_ID: 205
            },
            order: {
                ID: 'DESC'
            }
        }
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

{% include [Example Notes](../../../../_includes/examples.md) %}