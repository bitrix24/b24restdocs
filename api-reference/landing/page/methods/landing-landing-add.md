# Add page landing.landing.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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

The method `landing.landing.add` adds a page. It returns the `LID` of the created page or an error.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`unknown`](../../../data-types.md) | [Entity fields](../index.md) ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.add',
    		{
    			fields: {
    				TITLE: 'My first page!',
    				CODE: 'firstpage',
    				SITE_ID: 292,
    				ADDITIONAL_FIELDS: {
    					THEME_CODE: 'wedding'
    				}
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
                'landing.landing.add',
                [
                    'fields' => [
                        'TITLE' => 'My first page!',
                        'CODE' => 'firstpage',
                        'SITE_ID' => 292,
                        'ADDITIONAL_FIELDS' => [
                            'THEME_CODE' => 'wedding'
                        ]
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
        echo 'Error adding landing page: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.add',
        {
            fields: {
                TITLE: 'My first page!',
                CODE: 'firstpage',
                SITE_ID: 292,
                ADDITIONAL_FIELDS: {
                    THEME_CODE: 'wedding'
                }
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

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% note warning %}

When creating a page, the theme code of the page (`THEME_CODE: 'wedding'`) is passed. This is necessary for the page to be in the corresponding [color scheme](../color-themes.md). If this is not done, the page will be in the default theme.

{% endnote %}