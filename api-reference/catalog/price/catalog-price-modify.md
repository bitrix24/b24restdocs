# Modify Product Price Collection Elements catalog.price.modify

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameter specifications are missing
- no error response provided
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.price.modify(fields)
```

Method to modify elements of the product price collection.

{% note info "" %}

**Attention!** All entities that are not provided or do not have IDs specified will be deleted.

{% endnote %}

If the operation is successful, a [price resource](resource.md) is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields.product.id** 
[`string`](../../data-types.md) | Price number. ||
|| **fields.product.prices[]** 
[`list`](../../data-types.md) | Fields corresponding to the available list of [fields](catalog-price-get-fields.md). ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.price.modify',
    		{
    			fields: {
    				product: {
    					id: 8,
    					prices: [
    						{
    							catalogGroupId: 1,
    							currency: 'USD',
    							price: 2001,
    							quantityFrom: 1,
    							quantityTo: 2
    						},
    						{
    							catalogGroupId: 1,
    							currency: 'USD',
    							price: 2001,                
    							quantityFrom: 3,
    							quantityTo: 4
    						},
    						{
    							catalogGroupId: 1,
    							currency: 'USD',
    							price: 2001,                
    							quantityFrom: 5,
    							id: 122
    						},
    					]
    				},
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch(error)
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
                'catalog.price.modify',
                [
                    'fields' => [
                        'product' => [
                            'id'     => 8,
                            'prices' => [
                                [
                                    'catalogGroupId' => 1,
                                    'currency'       => 'USD',
                                    'price'          => 2001,
                                    'quantityFrom'   => 1,
                                    'quantityTo'     => 2
                                ],
                                [
                                    'catalogGroupId' => 1,
                                    'currency'       => 'USD',
                                    'price'          => 2001,
                                    'quantityFrom'   => 3,
                                    'quantityTo'     => 4
                                ],
                                [
                                    'catalogGroupId' => 1,
                                    'currency'       => 'USD',
                                    'price'          => 2001,
                                    'quantityFrom'   => 5,
                                    'id'             => 122
                                ],
                            ]
                        ],
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error modifying price: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.price.modify',
        {
            fields: {
                product: {
                    id: 8,
                    prices: [
                        {
                            catalogGroupId: 1,
                            currency: 'USD',
                            price: 2001,
                            quantityFrom: 1,
                            quantityTo: 2
                        },
                        {
                            catalogGroupId: 1,
                            currency: 'USD',
                            price: 2001,                
                            quantityFrom: 3,
                            quantityTo: 4
                        },
                        {
                            catalogGroupId: 1,
                            currency: 'USD',
                            price: 2001,                
                            quantityFrom: 5,
                            id: 122
                        },
                    ]
                },
            }
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

{% include [Examples Note](../../../_includes/examples.md) %}