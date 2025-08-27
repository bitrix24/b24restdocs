# Perform bulk deletion of inventory management documents catalog.document.deleteList

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.deleteList(documentIds)
```

Method for bulk deletion of inventory management documents.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **documentIds** 
[`array`](../../data-types.md)| An array of document IDs that need to be deleted. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.deleteList',
    		{
    			"documentIds": [
    				"110",
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
                'catalog.document.deleteList',
                [
                    'documentIds' => [
                        '110',
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
        echo 'Error deleting document list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.deleteList',
        {
            "documentIds": [
                "110",
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

{% include [Footnote about examples](../../../_includes/examples.md) %}