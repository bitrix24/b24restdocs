# Adding a New Block to the Page

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response not provided
- error response not provided
- links to yet-to-be-created pages are not specified (3 links in the Sites section)

{% endnote %}

{% endif %}

{% note info "landing.landing.addblock" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.addblock` adds a new block to the page. It returns the identifier of the new block or an error.

## Parameters

#|
|| **Method** | **Description** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier ||
|| **fields**
[`unknown`](../../../data-types.md) | Array of block fields, where currently only the following values are supported:
- **CODE** - symbolic code of the block. The block code can be obtained from the method [landing.block.getrepository](.). If a block registered by a partner through [landing.repo.register](.) is being added, the value for CODE must be passed as `repo_<ID>`, where `<ID>` is the identifier of that block.
- **AFTER_ID** - after which block (its ID) the new block should be added (if not specified, the block will be added at the beginning)
- **ACTIVE** - block activity (Y/N)
- **CONTENT** - entirely different content of the block (see notes for the method [landing.block.updatecontent](.)) ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.addblock',
    		{
    			lid: 351,
    			fields: {
    				CODE: '15.social'
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
                'landing.landing.addblock',
                [
                    'lid' => 351,
                    'fields' => [
                        'CODE' => '15.social'
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
        echo 'Error adding block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.addblock',
        {
            lid: 351,
            fields: {
                CODE: '15.social'
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