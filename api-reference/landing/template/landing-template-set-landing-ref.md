# Set Included Areas for the Page landing.template.setLandingRef

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.template.setLandingRef` sets the included areas for the page within a specific template (the page must already be linked to the template via the TPL_ID field). It will return *true* on success or an error.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the page. ||
|| **data**
[`unknown`](../../data-types.md) | Array of data to set (if the array is empty or not provided, the included areas will be reset). The keys of the array are the identifiers of the areas, and the values are the identifiers of the pages that need to be set as the area. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.template.setLandingRef',
    		{
    			id: 557,
    			data: {
    				1: 614,
    				2: 615,
    				3: 616
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
                'landing.template.setLandingRef',
                [
                    'id' => 557,
                    'data' => [
                        1 => 614,
                        2 => 615,
                        3 => 616
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
        echo 'Error setting landing reference: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.template.setLandingRef',
        {
            id: 557,
            data: {
                1: 614,
                2: 615,
                3: 616
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

{% include [Footnote on examples](../../../_includes/examples.md) %}