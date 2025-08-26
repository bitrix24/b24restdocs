# Adding a Block to My Blocks

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not specified
- examples missing
- success response missing
- error response missing
- links to yet-to-be-created pages not specified (1 link)

{% endnote %}

{% endif %}

{% note info "landing.landing.favoriteBlock" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.favoriteBlock` saves the existing block on the page to "My Blocks". It returns the identifier of the newly saved block.

{% note info %}

This method may be useful when deleting a block from saved ones.

{% endnote %}

## Parameters

#|
|| **Method** | **Description** ||
|| **lid**
[`unknown`](../../../data-types.md) | Identifier of the page. ||
|| **block**
[`unknown`](../../../data-types.md) | Identifier of the block. ||
|| **meta**
[`unknown`](../../../data-types.md) | Object containing information to save the block. Contains fields:
- **name** – name of the block;
- **section** – array of [categories](../../block/manifest.md) to save the block to;
- **preview** – image of the block. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.favoriteBlock',
    		{
    			lid: 11262,
    			block: 81827,
    			meta: {
    				name: 'My Block',
    				section: ['text', 'text_image'],
    				preview: 'https://mycdn.com/pic/1.jpg'
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch(error)
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
                'landing.landing.favoriteBlock',
                [
                    'lid'   => 11262,
                    'block' => 81827,
                    'meta'  => [
                        'name'    => 'My Block',
                        'section' => ['text', 'text_image'],
                        'preview' => 'https://mycdn.com/pic/1.jpg'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error favorite block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.favoriteBlock',
        {
            lid: 11262,
            block: 81827,
            meta: {
                name: 'My Block',
                section: ['text', 'text_image'],
                preview: 'https://mycdn.com/pic/1.jpg'
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
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}