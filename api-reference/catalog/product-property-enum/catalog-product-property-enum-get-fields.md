# Get Fields of List Property Values catalog.productPropertyEnum.getFields

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
catalog.productPropertyEnum.getFields()
```

The method returns the fields of list property values.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.productPropertyEnum.getFields',
    		{}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error().ex);
    	}
    	else
    	{
    		console.log(result);
    	}
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productPropertyEnum.getFields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error()->ex);
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting product property enum fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productPropertyEnum.getFields',
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

{% include [Note on examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** 
[`Type`](../../data-types.md) | **Description** | **Note** ||
|| **def** 
[`char`](../../data-types.md) | Is it the default value. | ||
|| **id** 
[`integer`](../../data-types.md) | Identifier of the value. | Read-only. ||
|| **propertyId^*^** 
[`integer`](../../data-types.md) | Identifier of the property. |  ||
|| **sort** 
[`integer`](../../data-types.md) | Sorting. | ||
|| **value^*^** 
[`string`](../../data-types.md) | Value. |  ||
|| **xmlId^*^** 
[`string`](../../data-types.md) | External identifier. | ||
|#
{% include [Note on parameters](../../../_includes/required.md) %}