# Update Product Property crm.product.property.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates an existing product property.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the product property ||
|| **fields**
[`array`](../../../data-types.md) | Field values for updating the product property.

To find out the required format for the fields, execute the method [crm.product.property.fields](./crm-product-property-fields.md) and check the format of the received values for these fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_property_id","fields":{"NAME":"New Property Name"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.property.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_property_id","fields":{"NAME":"New Property Name"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.product.property.update
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    var propertyName = prompt("Enter new name");
    BX24.callMethod(
        "crm.product.property.update",
        {
            id: id,
            fields:
            {
                "NAME": propertyName
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $id = 'your_property_id'; // Replace 'your_property_id' with the actual property ID
    $propertyName = 'New Property Name'; // Replace 'New Property Name' with the new name

    $result = CRest::call(
        'crm.product.property.update',
        [
            'id' => $id,
            'fields' => [
                'NAME' => $propertyName
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}