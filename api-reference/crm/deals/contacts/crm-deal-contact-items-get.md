# Get a set of contacts associated with a deal crm.deal.contact.items.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples are missing (in other languages)
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.contact.items.get` returns a set of contacts associated with the specified deal.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Identifier of the deal. ||
|#

{% include [Footnote about parameters](../../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.deal.contact.items.get",
    		{
    			id: id
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch(error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $dealId = 123; // Replace with the actual deal ID
        $result = $serviceBuilder
            ->getCRMScope()
            ->dealContact()
            ->itemsGet($dealId);

        foreach ($result->getDealContacts() as $item) {
            print("CONTACT_ID: " . $item->CONTACT_ID . "\n");
            print("SORT: " . $item->SORT . "\n");
            print("ROLE_ID: " . $item->ROLE_ID . "\n");
            print("IS_PRIMARY: " . $item->IS_PRIMARY . "\n");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.deal.contact.items.get",
        {
            id: id
        },
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

{% include [Footnote about examples](../../../../_includes/examples.md) %}

## Response in case of success

The result is returned as an array of objects, each containing the following fields:

#|
|| **Field** | **Description** ||
|| **CONTACT_ID** | Identifier of the contact ||
|| **SORT** | Sort index ||
|| **ROLE_ID** | Identifier of the role (reserved) ||
|| **IS_PRIMARY** | Flag for primary contact ||
|#