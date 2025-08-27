# Unregister AI Prompt ai.prompt.unregister

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`ai_admin`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `ai.prompt.unregister` removes a prompt.

#|
|| **Parameter** | **Description** ||
|| **code^*^**
[`unknown`](../../data-types.md) | Unique prompt code. Always has the prefix `rest_`. This code is set once during registration and cannot be changed afterwards ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'ai.prompt.unregister',
    		{
    			code: 'rest_joke_wolf'
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
                'ai.prompt.unregister',
                [
                    'code' => 'rest_joke_wolf'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error unregistering AI prompt: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'ai.prompt.unregister',
        {
            code: 'rest_joke_wolf'
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

- PHP CRest

    // example for PHP

{% endlist %}

{% include [Examples Notes](../../../_includes/examples.md) %}

## Success Response

## Error Response

## Typical use-cases and scenarios

- [{#T}](../../../tutorials/ai/add-joke-prompt.md)