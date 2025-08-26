# Export the site landing.site.fullExport

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.fullExport` exports the site and all its pages into a special array required for the method [landing.demos.register](../demos/landing-demos-register.md).

## Parameters

#|
|| **Parameter** | **Description** | **Available from** ||
|| **id**
[`unknown`](../../data-types.md) | Site identifier. | ||
|| **params**
[`unknown`](../../data-types.md) | Optional array, keys described in the example below. | ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.fullExport',
    		{
    			id: 326,
    			params: {
    				edit_mode: 'Y',
    				//scope: 'knowledge',//pass scope if required ([more details](.))
    				hooks_disable: ['B24BUTTON_CODE'],//codes of additional fields that should not be exported
    				code: 'myfirstsite',//symbolic code of the site
    				name: 'Auto Repair Shop Website',//name of the site (page)
    				description: 'Website for your auto service. Under the hood, everything you need.',//description of the site
    				preview: 'http://site.com/preview.jpg',//main preview image for the template list (recommended 280x115)
    				preview2x: 'http://site.com/preview.jpg',//enlarged preview image (recommended 560x230)
    				preview3x: 'http://site.com/preview.jpg'//retina-sized preview image (recommended 845x345)
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
                'landing.site.fullExport',
                [
                    'id' => 326,
                    'params' => [
                        'edit_mode'    => 'Y',
                        //scope: 'knowledge',//pass scope if required ([more details](.))
                        'hooks_disable' => ['B24BUTTON_CODE'],//codes of additional fields that should not be exported
                        'code'         => 'myfirstsite',//symbolic code of the site
                        'name'         => 'Auto Repair Shop Website',//name of the site (page)
                        'description'  => 'Website for your auto service. Under the hood, everything you need.',//description of the site
                        'preview'      => 'http://site.com/preview.jpg',//main preview image for the template list (recommended 280x115)
                        'preview2x'    => 'http://site.com/preview.jpg',//enlarged preview image (recommended 560x230)
                        'preview3x'    => 'http://site.com/preview.jpg'//retina-sized preview image (recommended 845x345)
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
        echo 'Error exporting site: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.fullExport',
        {
            id: 326,
            params: {
                edit_mode: 'Y',
                //scope: 'knowledge',//pass scope if required ([more details](.))
                hooks_disable: ['B24BUTTON_CODE'],//codes of additional fields that should not be exported
                code: 'myfirstsite',//symbolic code of the site
                name: 'Auto Repair Shop Website',//name of the site (page)
                description: 'Website for your auto service. Under the hood, everything you need.',//description of the site
                preview: 'http://site.com/preview.jpg',//main preview image for the template list (recommended 280x115)
                preview2x: 'http://site.com/preview.jpg',//enlarged preview image (recommended 560x230)
                preview3x: 'http://site.com/preview.jpg'//retina-sized preview image (recommended 845x345)
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

{% include [Footnote about examples](../../../_includes/examples.md) %}