# Update Content of the Block landing.block.updatecontent

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
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

The method `landing.block.updatecontent` updates the content of an already placed block on the page to any arbitrary content. To change the content part, it is recommended to use the method [landing.block.updatenodes](./landing-block-update-nodes.md). It will return _true_ on success, or an error.

{% note warning %}

- If the new block markup does not align with its current manifest, the block may become non-editable.
- Content is passed through a sanitizer, which may remove some suspicious attributes and tags.

{% endnote %}

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **content**
[`unknown`](../../../data-types.md) | New block content | ||
|| **designed**
[`unknown`](../../../data-types.md) | Optional, defaults to _false_. If _true_ is passed, the block will be considered locked from modification by the system's standard updater. | ||
|#

{% note info %}

The **style** attribute may be stripped by the built-in sanitizer. To bypass this, use the **bxstyle** attribute instead. When added, the system converts it to the standard style.

{% endnote %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.updatecontent',
    		{
    			lid: 625,
    			block: 11883,
    			content: '<h3>My super content</h3>'
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
                'landing.block.updatecontent',
                [
                    'lid'     => 625,
                    'block'   => 11883,
                    'content' => '<h3>My super content</h3>',
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
        echo 'Error updating block content: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.updatecontent',
        {
            lid: 625,
            block: 11883,
            content: '<h3>My super content</h3>'
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

{% include [Example Notes](../../../../_includes/examples.md) %}