# Conduct inventory management document catalog.document.conduct

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.conduct(id)
```

Method for conducting an inventory management document. If the operation is successful, `true` is returned in the response body.


## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md)| Identifier of the inventory management document. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.conduct',
    		{
    			id: 112
    		}
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
                'catalog.document.conduct',
                [
                    'id' => 112
                ]
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
        echo 'Error conducting document: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.conduct',
        {
            id: 112
        },
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

{% include [Footnote about examples](../../../_includes/examples.md) %}