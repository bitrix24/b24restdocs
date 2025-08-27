# Get a list of product fields for the inventory management document catalog.document.element.getFields

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

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```js
catalog.document.element.getFields()
```

The method returns a list of product fields for the inventory management document.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.element.getFields',
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.document.element.getFields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting document fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.element.getFields',
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

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

## Returned fields

#|
|| **Field** | **Description** | **Note** ||
|| **amount** 
[`double`](../../../data-types.md) | Quantity. | ||
|| **docId** 
[`integer`](../../../data-types.md) | Document identifier. | Immutable field. ||
|| **elementId** 
[`integer`](../../../data-types.md) | Product identifier [catalog.product.list](../../../catalog/product/catalog-product-list.md). | Immutable field. ||
|| **id** 
[`integer`](../../../data-types.md) | Document product identifier. | Read-only. ||
|| **purchasingPrice** 
[`double`](../../../data-types.md) | Purchase price. | ||
|| **storeFrom** 
[`integer`](../../../data-types.md) | Sender warehouse. | ||
|| **storeTo** 
[`integer`](../../../data-types.md) | Recipient warehouse. | ||
|#

{% include [Footnote about parameters](../../../../_includes/required.md) %}