# Change parameters of entity.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.update` method updates the parameters of the data storage. The user must have management rights (**X**) for the storage. The user cannot revoke their own management rights.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Required. String identifier of the storage being updated. ||
|| **NAME**
[`string`](../../data-types.md) | New name of the storage. ||
|| **ACCESS**
[`unknown`](../../data-types.md) | Description of the new set of access permissions for the storage. 
It should be in the form of an associative array, where the keys are the identifiers of access permissions, and the values are **R** (read), **W** (write), or **X** (manage). ||
|| **ENTITY_NEW**
[`string`](../../data-types.md) | New string identifier of the storage. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.update',
    		{
    			'ENTITY': 'dish',
    			'ACCESS': {
    				U1:'W',
    				AU:'R'
    			}
    		}
    	);
    	
    	const result = response.getData().result;
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
                'entity.update',
                [
                    'ENTITY' => 'dish',
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
        echo 'Error updating entity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'entity.update',
        {
            'ENTITY': 'dish',
            'ACCESS': {
                U1:'W',
                AU:'R'
            }
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}