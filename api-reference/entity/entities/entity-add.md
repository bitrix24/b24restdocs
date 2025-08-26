# Create Data Storage entity.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.add` method creates a data storage. Before creating, you can check for the existence of the storage using [entity.get](./entity-get.md).

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Required. String identifier of the storage, unique for this application (maximum length - 13 characters). ||
|| **NAME**^*^
[`string`](../../data-types.md) | Required. Name of the storage. ||
|| **ACCESS**
[`unknown`](../../data-types.md) | Description of access permissions for the storage. 
It should be in the form of an associative array, where the keys are the identifiers of access permissions, and the values are **R** (read), **W** (write), or **X** (manage). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

The Creator of the storage automatically receives **X** permission.

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.add',
    		{
    			'ENTITY': 'dish',
    			'NAME': 'Dishes',
    			'ACCESS': {
    				U1:'W',
    				AU:'R'
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Created element with ID:', result);
    	// Your required data processing logic
    	processResult(result);
    }
    catch( error )
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
                'entity.add',
                [
                    'ENTITY' => 'dish',
                    'NAME'   => 'Dishes',
                    'ACCESS' => [
                        'U1' => 'W',
                        'AU' => 'R'
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
        echo 'Error adding entity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'entity.add',
        {
            'ENTITY': 'dish',
            'NAME': 'Dishes',
            'ACCESS': {
                U1:'W',
                AU:'R'
            }
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}