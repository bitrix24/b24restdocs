# Add Page by Template landing.landing.addByTemplate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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

The method `landing.landing.addByTemplate` adds a page based on a template (a list of templates that the user sees before creating the page). It returns the `ID` of the created page or an error.

You cannot influence the fields of the created page; for that, you can use [landing.landing.add](./landing-landing-add.md).

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **siteId**
[`unknown`](../../../data-types.md) | `ID` of the site where the page needs to be created. ||
|| **code**
[`unknown`](../../../data-types.md) | Identifier of the template to be used for creation. You can get the list of templates using the method [landing.demos.getPageList](../../demos/landing-demos-get-page-list.md). ||
|| **fields**
[`unknown`](../../../data-types.md) | Optional. You can pass an array of fields for the created page. Currently, only the keys TITLE and DESCRIPTION are supported. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.addByTemplate',
    		{
    			siteId: 870,
    			code: 'agency',
    			fields: {
    				TITLE: 'Page Title',
    				DESCRIPTION: 'Page Description'
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.info(result);
    	}
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.landing.addByTemplate',
                [
                    'siteId' => 870,
                    'code' => 'agency',
                    'fields' => [
                        'TITLE' => 'Page Title',
                        'DESCRIPTION' => 'Page Description'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding landing by template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.addByTemplate',
        {
            siteId: 870,
            code: 'agency',
            fields: {
                TITLE: 'Page Title',
                DESCRIPTION: 'Page Description'
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}