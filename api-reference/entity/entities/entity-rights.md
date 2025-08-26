# Get or Change Access Permissions for entity.rights

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.rights` method retrieves or modifies access permissions for the storage. It returns the current set of access permissions.

To change the set of access permissions, the user must have management rights (**X**) for the storage. A user cannot revoke their own management rights.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Required. String identifier of the storage being updated. ||
|| **ACCESS**
[`unknown`](../../data-types.md) | Description of the new set of access permissions for the storage. 
It should be in the form of an associative array, where the keys are the identifiers of the access permissions, and the values are **R** (read), **W** (write), or **X** (manage). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.rights',
    		{
    			'ENTITY': 'dish'
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
                'entity.rights',
                [
                    'ENTITY' => 'dish'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling entity rights: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'entity.rights',
        {
            'ENTITY': 'dish'
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}

Response

> 200 OK
```json
{"result":
    {
        "AU":"R",
        "U254":"W",
        "D115":"W",
        "U255":"W",
        "U260":"W"
    }
}
```