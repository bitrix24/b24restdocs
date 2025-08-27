# Conduct a group processing of inventory management documents catalog.document.conductList

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
catalog.document.conductList(documentIds)
```

Method for group processing of inventory management documents.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **documentIds**
[`array`](../../data-types.md)| An array of document identifiers that need to be processed. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.conductList',
    		{
    			"documentIds": [
    				"114",
    				"112"
    			]
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error.ex);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.document.conductList',
                [
                    'documentIds' => [
                        '114',
                        '112'
                    ]
                ]
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
        echo 'Error conducting document list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.conductList',
        {
            "documentIds": [
                "114",
                "112"
            ]
        },
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