# Change Folder in landing.site.updateFolder

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
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

The method `landing.site.updateFolder` changes the folder in the site.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **siteId**
[`unknown`](../../data-types.md) | Identifier of the site.

{% note warning %}

Write permissions are required for the specified site.

{% endnote %}

 | ||
|| **folderId**
[`unknown`](../../data-types.md) | Identifier of the folder in the site. | ||
|| **fields**
[`unknown`](../../data-types.md) | Folder fields: 
- ACTIVE – folder activity (Y/N). By default, it is created inactive;
- TITLE – title (name) of the folder;
- INDEX_ID – identifier of the page within the folder that needs to be set as the index page of the folder;
- CODE – symbolic code of the folder (part of the folder page URL). By default, it is transliterated from the folder name. | ||
|#

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.updateFolder',
    		{
    			siteId: 1817,
    			folderId: 736,
    			fields: {
    				TITLE: 'Updated Folder'
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
                'landing.site.updateFolder',
                [
                    'siteId'  => 1817,
                    'folderId' => 736,
                    'fields'  => [
                        'TITLE' => 'Updated Folder'
                    ],
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
        echo 'Error updating folder: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.updateFolder',
        {
            siteId: 1817,
            folderId: 736,
            fields: {
                TITLE: 'Updated Folder'
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