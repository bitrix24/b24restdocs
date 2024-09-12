# Get Fields of Product Property Features or Trade Offers catalog.productPropertyFeature.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing response in case of error
- missing response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
catalog.productPropertyFeature.getFields()
```

The method returns the fields of product property features or trade offers.

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    'catalog.productPropertyFeature.getFields',
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
{% include [Footnote on examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||


|| **featureId^*^**
[`string`](../../data-types.md)| Parameter code. |  ||
|| **id**
[`integer`](../../data-types.md)| Identifier of the property feature. | Read-only. ||
|| **isEnabled^*^**
[`char`](../../data-types.md)| Is the parameter enabled. |  ||
|| **moduleId^*^**
[`string`](../../data-types.md)| Module identifier. |  ||
|| **propertyId^*^**
[`integer`](../../data-types.md)| Identifier of the property. |  ||
|#
{% include [Footnote on parameters](../../../_includes/required.md) %}