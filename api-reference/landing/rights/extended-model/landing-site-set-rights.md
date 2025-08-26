# Set access permissions for the site landing.site.setRights

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `landing.site.setRights` sets access permissions for the site. It will return *true* or an error. The method is available only to the portal administrator, and in the cloud, it is also limited to paid plans.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Site identifier. ||
|| **rights**
[`unknown`](../../../data-types.md) | An object with permissions, where the keys are unique identifiers (user, department, group, ...), and the values are allowed operations:
- **denied** – access denied
- **read** – read
- **edit** – edit (page content)
- **sett** – change settings
- **public** – publish
- **delete** – delete (to trash, and restore from trash)

Permissions are independent and can be granted selectively. For example, a user may have only the right to publish without the ability to make any changes.

The following values can be used as keys:
- **SG<X>** - workgroup
- **U<X>** - user
- **DR<X>** - department, including subdivisions
- **UA** - all authorized users
- **G<X>** - user group ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.setRights',
    		{
    			id: 645,
    			rights: {
    				'U3': [
    					'edit', 'delete'
    				],
    				'U1': [
    					'edit', 'sett'
    				]
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
                'landing.site.setRights',
                [
                    'id'     => 645,
                    'rights' => [
                        'U3' => ['edit', 'delete'],
                        'U1' => ['edit', 'sett'],
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
        echo 'Error setting site rights: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.setRights',
        {
            id: 645,
            rights: {
                'U3': [
                    'edit', 'delete'
                ],
                'U1': [
                    'edit', 'sett'
                ]
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