# Get Fields for Deal-Contact Connection crm.deal.contact.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.contact.fields` returns the description of the fields used by the methods of the `crm.deal.contact.*` family, namely [crm.deal.contact.items.get](./crm-deal-contact-items-get.md), [crm.deal.contact.items.set](./crm-deal-contact-items-set.md), [crm.deal.contact.add](./crm-deal-contact-add.md), etc.

Without parameters.

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.deal.contact.fields",
    		{}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch(error)
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
                'crm.deal.contact.fields',
                []
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
        echo 'Error fetching deal contact fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.deal.contact.fields",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index (number). Determines the order in which linked contacts will be displayed in the deal. ||
|| **IS_PRIMARY**
[`char`](../../../data-types.md) | [Y/N] Indicates whether the binding is primary. There is always a primary contact in the deal. For it, `IS_PRIMARY=Y`, for others `IS_PRIMARY=N`. ||
|| **CONTACT_ID**
[`integer`](../../../data-types.md) | Identifier of the contact linked to the deal (number). ||
|#