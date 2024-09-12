# Get the list of templates landing.template.getlist

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.template.getlist` retrieves a list of templates.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **params**
[`unknown`](../../data-types.md) | Optional array with optional keys: **select**, **filter**, **order**, **group**, which contain values from the [main fields](./fields.md) of the entity. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.template.getlist',
    {
        params: {
            select: [
                'ID', 'TITLE'
            ],
            filter: {
                '>ID': 0
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

{% include [Footnote about examples](../../../_includes/examples.md) %}