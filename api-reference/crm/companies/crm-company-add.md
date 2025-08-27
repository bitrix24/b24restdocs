# Create a New Company crm.company.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- required parameters not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.add` creates a new company.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`array`](../../data-types.md) | Set of fields - an array in the form array("field"=>"value"[, ...]), containing the values of the company fields. 

{% note info %}

To find out the required format of the fields, execute the method [crm.company.fields](./crm-company-fields.md) and check the format of the received values for these fields.

{% endnote %}
 ||
|| **params**
[`array`](../../data-types.md) | Set of parameters. REGISTER_SONET_EVENT - register the event of adding a company in the live feed. A notification will also be sent to the person responsible for the company. ||
|#

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.company.add",
    		{
    			fields:
    			{
    				"TITLE": "LLC Smith",
    				"COMPANY_TYPE": "CUSTOMER",
    				"INDUSTRY": "MANUFACTURING",
    				"EMPLOYEES": "EMPLOYEES_2",
    				"CURRENCY_ID": "USD",
    				"REVENUE" : 3000000,
    				"LOGO": { "fileData": document.getElementById('logo') },
    				"OPENED": "Y",
    				"ASSIGNED_BY_ID": 1,
    				"PHONE": [ { "VALUE": "555888", "VALUE_TYPE": "WORK" } ]     
    			},
    			params: { "REGISTER_SONET_EVENT": "Y" }        
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info("Company created with ID " + result);
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
                'crm.company.add',
                [
                    'fields' => [
                        'TITLE'         => 'LLC Smith',
                        'COMPANY_TYPE'  => 'CUSTOMER',
                        'INDUSTRY'      => 'MANUFACTURING',
                        'EMPLOYEES'     => 'EMPLOYEES_2',
                        'CURRENCY_ID'   => 'USD',
                        'REVENUE'       => 3000000,
                        'LOGO'          => ['fileData' => $_POST['logo']],
                        'OPENED'        => 'Y',
                        'ASSIGNED_BY_ID' => 1,
                        'PHONE'         => [['VALUE' => '555888', 'VALUE_TYPE' => 'WORK']],
                    ],
                    'params' => ['REGISTER_SONET_EVENT' => 'Y'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Company created with ID ' . $result;
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.company.add",
        {
            fields:
            {
                "TITLE": "LLC Smith",
                "COMPANY_TYPE": "CUSTOMER",
                "INDUSTRY": "MANUFACTURING",
                "EMPLOYEES": "EMPLOYEES_2",
                "CURRENCY_ID": "USD",
                "REVENUE" : 3000000,
                "LOGO": { "fileData": document.getElementById('logo') },
                "OPENED": "Y",
                "ASSIGNED_BY_ID": 1,
                "PHONE": [ { "VALUE": "555888", "VALUE_TYPE": "WORK" } ]     
            },
            params: { "REGISTER_SONET_EVENT": "Y" }        
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Company created with ID " + result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-company.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-company-with-requisite.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-deal-with-choice-of-requisite.md)