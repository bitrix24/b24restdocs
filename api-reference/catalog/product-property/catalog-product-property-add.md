# Add Product or Variation Property catalog.productProperty.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of success
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.productProperty.add(fields)
```

This method adds a property for products or variations.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-product-property-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.productProperty.add',
    		{
    			fields: {
    				name: "Size",
    				active: "Y",
    				code: "SIZE1",
    				iblockId: 16,
    				propertyType: "L",
    				isRequired: "N",
    				listType: "L",
    				filtrable: "Y",
    				multiple: "N"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    		console.error(result.error().ex);
    	else
    		console.log(result);
    }
    catch( error )
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
                'catalog.productProperty.add',
                [
                    'fields' => [
                        'name'          => "Size",
                        'active'        => "Y",
                        'code'          => "SIZE1",
                        'iblockId'      => 16,
                        'propertyType'  => "L",
                        'isRequired'    => "N",
                        'listType'      => "L",
                        'filtrable'     => "Y",
                        'multiple'      => "N",
                    ],
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
        echo 'Error adding product property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productProperty.add',
        {
            fields: {
                name: "Size",
                active: "Y",
                code: "SIZE1",
                iblockId: 16,
                propertyType: "L",
                isRequired: "N",
                listType: "L",
                filtrable: "Y",
                multiple: "N"
            }
        },
        function(result) {
            if (result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}