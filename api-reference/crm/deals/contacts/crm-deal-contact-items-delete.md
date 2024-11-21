# Delete the set of contacts associated with the specified deal crm.deal.contact.items.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.contact.items.delete` clears the set of contacts associated with the specified deal.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Identifier of the deal. ||
|#

{% include [Parameter notes](../../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.deal.contact.items.delete",
        {
            id: id
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- B24-PHP-SDK

    ```php
    try {
        $dealId = 123; // Replace with the actual deal ID you want to delete contacts from
        $result = $serviceBuilder->getCRMScope()->dealContact()->itemsDelete($dealId);

        if ($result->isSuccess()) {
            print("Successfully deleted contacts from deal ID: $dealId");
        } else {
            print("Failed to delete contacts. Result: " . json_encode($result));
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

{% endlist %}

{% include [Parameter notes](../../../../_includes/required.md) %}