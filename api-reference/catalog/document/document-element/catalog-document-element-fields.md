# Get the list of fields for inventory document items catalog.document.element.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

{% note info "Attention!" %}

This method has been deprecated since version **22.400.0**. It is recommended to use the method [catalog.document.element.getFields](./catalog-document-element-get-fields.md).

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.element.fields()
```

This method returns a list of fields for inventory document items.

## Parameters

No parameters.

## Examples

{% list tabs %}

- js
  
    ```js
    BX24.callMethod(
        'catalog.document.element.fields',
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- php
  
    ```php
    $result = CRest::call(
        'catalog.document.element.fields'
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```
{% endlist %}

{% include [Examples note](../../../../_includes/examples.md) %}

## Returned fields

#|
|| **Field** | **Description** | **Note** ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier. | ||
|| **docId^*^** 
[`integer`](../../../data-types.md) | Document identifier. |  ||
|| **storeFrom** 
[`integer`](../../../data-types.md) | Source warehouse. | ||
|| **storeTo^*^** 
[`integer`](../../../data-types.md) | Destination warehouse. | ||
|| **elementId^*^** 
[`integer`](../../../data-types.md) | Product identifier [catalog.product.list](../../../catalog/product/catalog-product-list.md). | ||
|| **amount^*^** 
[`double`](../../../data-types.md) | Quantity. |  ||
|| **purchasingPrice^*^** 
[`double`](../../../data-types.md) | Purchase price. | ||
|#