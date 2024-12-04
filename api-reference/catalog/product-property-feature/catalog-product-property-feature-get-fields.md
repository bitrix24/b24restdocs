# Get Product Property Feature Parameter Fields or Variations catalog.productPropertyFeature.getFields

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
catalog.productPropertyFeature.getFields()
```

The method returns the fields of product property parameters or variations.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```js
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

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **featureId^*^**
[`string`](../../data-types.md)| Parameter code. |  ||
|| **id**
[`integer`](../../data-types.md)| Property parameter identifier. | Read-only. ||
|| **isEnabled^*^**
[`char`](../../data-types.md)| Whether the parameter is enabled. |  ||
|| **moduleId^*^**
[`string`](../../data-types.md)| Module identifier. |  ||
|| **propertyId^*^**
[`integer`](../../data-types.md)| Property identifier. |  ||
|#
{% include [Footnote about parameters](../../../_includes/required.md) %}