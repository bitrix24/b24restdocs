# Remove Related Entities landing.landing.removeEntities

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.landing.removeEntities` removes related entities of the landing - blocks and block images.

{% note warning %}

When blocks are deleted, the associated images are removed in any case. However, there may be situations where it is necessary to delete images independently of the blocks to clean up junk. Use this method in that case.

{% endnote %}

## Parameters

#|
|| **Parameters** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Identifier of the landing. ||
|| **data**
[`unknown`](../../../data-types.md) | Associative array where the key **blocks** contains the blocks to be deleted, and the key **images** contains block-image pairs for which images need to be deleted (blocks are not deleted in this case). ||
|#

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.removeEntities',
    		{
    			lid: 648,
    			data: {
    				blocks: [12167, 123],
    				images: [
    					{
    						block: 12269,
    						image: 6866
    					},
    					{
    						block: 12268,
    						image: 6861
    					}
    				]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'landing.landing.removeEntities',
                [
                    'lid' => 648,
                    'data' => [
                        'blocks' => [12167, 123],
                        'images' => [
                            [
                                'block' => 12269,
                                'image' => 6866
                            ],
                            [
                                'block' => 12268,
                                'image' => 6861
                            ]
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error removing entities: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.removeEntities',
        {
            lid: 648,
            data: {
                blocks: [12167, 123],
                images: [
                    {
                        block: 12269,
                        image: 6866
                    },
                    {
                        block: 12268,
                        image: 6861
                    }
                ]
            }
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    // In this example, we are removing blocks with IDs 12167, 123, as well as image 6866 (from block 12269) and image 6861 (from block 12268).
    // All entities are located in landing 648.
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}