# Add estimate crm.quote.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing (should include three examples - curl, js, php)
- error response missing
- success response missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method Development Stopped" %}

The method `crm.quote.add` continues to function, but there is a more relevant alternative [crm.item.add](../universal/crm-item-add.md).

{% endnote %}

The method `crm.quote.add` creates a new estimate. If you need to specify any details of the buyer/seller in the estimate (since there may be several for a company), use the method [crm.requisite.link.register](../requisites/links/crm-requisite-link-register.md).

The created estimate must include the seller and buyer companies:
- `COMPANY_ID`, if the buyer is a company or `CONTACT_ID`, if the buyer is a contact.
- `MYCOMPANY_ID` - seller. 

The identifiers specified in **crm.requisite.link.register** and in the created estimate must correspond to the buyer and seller.

#|
||  **Parameter** / **Type**| **Description** ||
|| **fields**
[`unknown`](../../data-types.md) | Set of fields - an array of the form `array("field"=>"value"[, ...])`, containing the values of the estimate fields, where "field" can take values returned by the method [crm.quote.fields](./crm-quote-fields.md).

{% note info %}

To find out the required format of the fields, execute the method [crm.quote.fields](./crm-quote-fields.md) and check the format of the returned values for these fields. 

{% endnote %}

||
|#

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.quote.add",
    		{
    			fields:
    			{
    				"TITLE": "Draft",
    				"STATUS_ID": "DRAFT",
    				"OPENED": "Y",
    				"ASSIGNED_BY_ID": 1,
    				"CURRENCY_ID": "USD",
    				"OPPORTUNITY": 5000,
    				"COMPANY_ID": 1,
    				"COMMENTS": "New estimate.",
    				"BEGINDATE": "2016-03-01T12:00:00",
    				"CLOSEDATE": "2016-04-01T12:00:00"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info("Estimate created with ID " + result);
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
                'crm.quote.add',
                [
                    'fields' => [
                        'TITLE'          => 'Draft',
                        'STATUS_ID'      => 'DRAFT',
                        'OPENED'         => 'Y',
                        'ASSIGNED_BY_ID' => 1,
                        'CURRENCY_ID'    => 'USD',
                        'OPPORTUNITY'    => 5000,
                        'COMPANY_ID'     => 1,
                        'COMMENTS'       => 'New estimate.',
                        'BEGINDATE'      => '2016-03-01T12:00:00',
                        'CLOSEDATE'      => '2016-04-01T12:00:00',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Estimate created with ID ' . $result;
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating quote: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.quote.add",
        {
            fields:
            {
                "TITLE": "Draft",
                "STATUS_ID": "DRAFT",
                "OPENED": "Y",
                "ASSIGNED_BY_ID": 1,
                "CURRENCY_ID": "USD",
                "OPPORTUNITY": 5000,
                "COMPANY_ID": 1,
                "COMMENTS": "New estimate.",
                "BEGINDATE": "2016-03-01T12:00:00",
                "CLOSEDATE": "2016-04-01T12:00:00"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Estimate created with ID " + result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}