# Set Role Rights for Site Lists landing.role.setRights

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types not specified
- parameter requirements not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

{% note info "" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Execution Rights**: `administrator`

{% endnote %}

The method `landing.role.setRights` sets the necessary rights within a role for site lists. All other sites not specified in the incoming array are considered unlinked from the role.

The keys of the array are the site identifiers, and the values are arrays of available operations (a zero key means default access for the role):

- **denied** - all access denied,
- **read** – read (this right is automatically granted by the system when any other right except denied is specified),
- **edit** – modify (page content),
- **sett** – change settings,
- **public** – publish,
- **delete** – delete (to trash, and restore from trash).

## Parameters

#|
|| **Parameters** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Role identifier. ||
|| **rights**
[`unknown`](../../../data-types.md) | Array of sites for rights binding. See example. ||
|| **additional**
[`unknown`](../../../data-types.md) | Optionally, an array with additional rights can be passed, specifying who is allowed within the role:
- **menu24** – whether to show the "Sites" / "Stores" menu item in cloud Bitrix24 for this role
- **create** – whether to allow creating sites within the role ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.role.setRights',
    		{
    			id: 11,
    			rights: {
    				'0': ['read'],
    				'66': ['read','edit','sett']
    			},
    			additional: ['menu24', 'create']
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
                'landing.role.setRights',
                [
                    'id' => 11,
                    'rights' => [
                        '0'  => ['read'],
                        '66' => ['read', 'edit', 'sett']
                    ],
                    'additional' => ['menu24', 'create']
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
        echo 'Error setting role rights: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.role.setRights',
        {
            id: 11,
            rights: {
                '0': ['read'],
                '66': ['read','edit','sett']
            },
            additional: ['menu24', 'create']
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}