# Get site folders landing.site.getFolders

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getFolders` retrieves the site folders.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **siteId**
[`unknown`](../../data-types.md) | Site identifier. 

{% note warning %}

Write access permission is required for the specified site. 

{% endnote %}

| ||
|| **filter**
[`unknown`](../../data-types.md) | Optional filter. Can accept the following fields:
- ACTIVE – folder activity (Y/N). By default, it is created inactive;
- DELETED – folder deleted (Y/N). By default, non-deleted folders are returned;
- PARENT_ID – identifier of the parent folder;
- TITLE – folder title;
- INDEX_ID – identifier of the folder's index page;
- CODE – symbolic code of the folder;
- CREATED_BY_ID – identifier of the user who created the folder;
- MODIFIED_BY_ID – identifier of the user who modified the folder; | ||
|#

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.getFolders',
    		{
    			siteId: 1817,
    			filter: {
    				TITLE: 'Modified folder'
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
                'landing.site.getFolders',
                [
                    'siteId' => 1817,
                    'filter' => [
                        'TITLE' => 'Modified folder'
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
        echo 'Error getting site folders: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getFolders',
        {
            siteId: 1817,
            filter: {
                TITLE: 'Modified folder'
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