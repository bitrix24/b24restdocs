# Add Card with Modified Content landing.block.addcard

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- corrections needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.addcard` fully replicates the functionality of [landing.block.clonecard](./landing-block-clone-card.md) but allows you to insert a card with modified content right away.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **selector**
[`unknown`](../../../data-types.md) | [Card selector](../manifest.md#key-cards), taken from the manifest, with the added card identifier.
For example: `.landing-block-card@0`. The 0 at the end indicates that we are affecting the first card in order. | ||
|| **content**
[`unknown`](../../../data-types.md) | Content of the new card. | ||
|#

{% note warning %}

Please note that once you clone a card, their counters change.

{% endnote %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.addCard',
    		{
    			lid: 634,
    			block: 12079,
    			selector: '.landing-block-node-menu-list-item@0',
    			content: '<li class="landing-block-node-menu-list-item nav-item g-mx-30--lg g-mb-7 g-mb-0--lg">' + '<a href="#about" class="landing-block-node-menu-list-item-link nav-link g-color-white p-0">New card item</a>' + '</li>'
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
                'landing.block.addCard',
                [
                    'lid'      => 634,
                    'block'    => 12079,
                    'selector' => '.landing-block-node-menu-list-item@0',
                    'content'  => '<li class="landing-block-node-menu-list-item nav-item g-mx-30--lg g-mb-7 g-mb-0--lg">' . '<a href="#about" class="landing-block-node-menu-list-item-link nav-link g-color-white p-0">New card item</a>' . '</li>'
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
        echo 'Error adding card: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.addCard',
        {
            lid: 634,
            block: 12079,
            selector: '.landing-block-node-menu-list-item@0',
            content: '<li class="landing-block-node-menu-list-item nav-item g-mx-30--lg g-mb-7 g-mb-0--lg">' + '<a href="#about" class="landing-block-node-menu-list-item-link nav-link g-color-white p-0">New card item</a>' + '</li>'
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

{% include [Examples note](../../../../_includes/examples.md) %}