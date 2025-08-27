# Update an Existing Company crm.company.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.update` updates an existing company.

{% note warning %}

It is strongly recommended to pass the complete set of address fields when updating the address in the update method. The specifics of updating address fields are described [here](../data-types.md).

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Company identifier. ||
|| **fields**
[`unknown`](../../data-types.md) | [Set of fields](./crm-company-add.md) - an array in the form array("field to update"=>"value"[, ...]), where "field to update" can take values returned by the method [crm.company.fields](./crm-company-fields.md). 

{% note info %}

To find out the required format of the fields, execute the method [crm.company.fields](./crm-company-fields.md) and check the format of the returned values for these fields.

{% endnote %}

 ||
|| **params**
[`unknown`](../../data-types.md) | Set of parameters. `REGISTER_SONET_EVENT` - register an event of the company change in the live feed. A notification will also be sent to the person responsible for the company. ||
|#

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.company.update",
    		{
    			id: id,
    			fields:
    			{
    				"CURRENCY_ID": "USD",
    				"REVENUE" : 500000,
    				"EMPLOYEES": "EMPLOYEES_3"
    			},
    			params: { "REGISTER_SONET_EVENT": "Y" }
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
    $id = readline("Enter ID");
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.update',
                [
                    'id' => $id,
                    'fields' => [
                        'CURRENCY_ID' => 'USD',
                        'REVENUE' => 500000,
                        'EMPLOYEES' => 'EMPLOYEES_3',
                    ],
                    'params' => ['REGISTER_SONET_EVENT' => 'Y'],
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
        echo 'Error updating company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.company.update",
        {
            id: id,
            fields:
            {
                "CURRENCY_ID": "USD",
                "REVENUE" : 500000,
                "EMPLOYEES": "EMPLOYEES_3"
            },
            params: { "REGISTER_SONET_EVENT": "Y" }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}