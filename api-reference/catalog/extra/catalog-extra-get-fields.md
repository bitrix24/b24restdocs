# Get Markup Fields catalog.extra.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
catalog.extra.getFields()
```

The method returns markup fields.

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    'catalog.extra.getFields',
    {},
    function(result)
    {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Footnote about examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **id**
[`integer`](../../data-types.md) | Markup identifier | Read-only ||
|| **name^*^**
[`string`](../../data-types.md) | Name |  ||
|| **percentage^*^**
[`string`](../../data-types.md) | Markup amount |  ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}