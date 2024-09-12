# Get the list of sites landing.site.getList

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getList` retrieves a list of sites.

{% note warning %}

Please note that pages marked as deleted do not appear in the selections. To explicitly retrieve them, you need to specify the key `DELETED` with a value of Y or N when filtering.

{% endnote %}

## Parameters

#|
|| **Method** | **Description** | **Version** ||
|| **params**
[`unknown`](../../data-types.md) | An optional array with optional keys: **select**, **filter**, **order**, **group**, which contain values from the [main fields](./base-fields.md) of the entity. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.getList',
    {
        params: {
            select: [
                'ID', 'TITLE', 'DOMAIN.DOMAIN'
            ],
            filter: {
                TITLE: '%services%'
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

{% include [Footnote on examples](../../../_includes/examples.md) %}