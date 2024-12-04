# Delete deal crm.activity.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.delete` deletes a deal.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the deal. ||
|#

## Examples

{% list tabs %}

- JS
    
    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.activity.delete",
        { id: id },
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
        $itemId = 123; // Replace with the actual item ID to delete
        $result = $serviceBuilder->getCRMScope()->activity()->delete($itemId);

        if ($result->isSuccess()) {
            print("Item deleted successfully.");
        } else {
            print("Failed to delete item.");
        }
    } catch (Throwable $e) {
        print("Error occurred: " . $e->getMessage());
    }
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}