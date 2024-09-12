# Delete Product Property crm.product.property.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method deletes a product property.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Product identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_property_id"}' \ # Replace 'your_property_id' with the actual property ID
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.property.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_property_id","auth":"**put_access_token_here**"}' \ # Replace 'your_property_id' with the actual property ID
    https://**put_your_bitrix24_address**/rest/crm.product.property.delete
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.product.property.delete",
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

- PHP

    ```php
    require_once('crest.php');

    $id = 'your_property_id'; // Replace 'your_property_id' with the actual property ID

    $result = CRest::call(
        'crm.product.property.delete',
        [
            'id' => $id
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}