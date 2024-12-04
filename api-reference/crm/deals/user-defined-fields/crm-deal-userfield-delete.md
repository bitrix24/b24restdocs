# Delete Custom Field of Deals crm.deal.userfield.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

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

The method `crm.deal.userfield.delete` removes a custom field from deals.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Identifier of the custom field. ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.deal.userfield.delete",
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

- PHP (B24PhpSdk)

    ```php
    try {
        $userfieldId = 123; // Replace with the actual userfield ID you want to delete
        $result = $serviceBuilder
            ->getCRMScope()
            ->dealUserfield()
            ->delete($userfieldId);

        if ($result->isSuccess()) {
            print("Userfield deleted successfully.");
        } else {
            print("Failed to delete userfield.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

{% endlist %}

{% include [Note on examples](../../../../_includes/examples.md) %}