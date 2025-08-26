# Remove Card Method landing.block.removecard

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- revisions needed for standard writing
- parameter types not specified
- parameter requirements not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.removecard` removes a block card. It returns *_true_* or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **selector**
[`unknown`](../../../data-types.md) | [Card selector](../manifest.md#key-cards) taken from the manifest, with the card identifier added.
For example: '.landing-block-card@0'. The 0 at the end indicates that we are affecting the first card in order. | ||
|#

{% note warning %}

Please note that once you have removed a card, their counters have changed.

{% endnote %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.removecard',
    		{
    			lid: 311,
    			block: 6057,
    			selector: '.landing-block-card@0'
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
                'landing.block.removecard',
                [
                    'lid'      => 311,
                    'block'    => 6057,
                    'selector' => '.landing-block-card@0',
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
        echo 'Error removing card: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.removecard',
        {
            lid: 311,
            block: 6057,
            selector: '.landing-block-card@0'
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