# Add site landing.site.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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

The method `landing.site.add` adds a site. It returns the `ID` of the created site or an error.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **fields**
[`unknown`](../../data-types.md) | [Entity fields](./base-fields.md) | ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.add',
    		{
    			fields: {
    				TITLE: 'My first site!',
    				CODE: 'firstsite',
    				DOMAIN_ID: 'my.bitrix24.site'
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
                'landing.site.add',
                [
                    'fields' => [
                        'TITLE'     => 'My first site!',
                        'CODE'      => 'firstsite',
                        'DOMAIN_ID' => 'my.bitrix24.site',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding site: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.add',
        {
            fields: {
                TITLE: 'My first site!',
                CODE: 'firstsite',
                DOMAIN_ID: 'my.bitrix24.site'
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