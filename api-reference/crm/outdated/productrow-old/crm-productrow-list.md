# Get a List of Order Properties crm.productrow.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`crm.item.productrow.list`](../../universal/product-rows/crm-item-productrow-list.md)

{% endnote %}

The method returns a list of product items based on the filter. It is an implementation of the list method for product items. The owner of the product items is determined by the required fields `OWNER_TYPE` and `OWNER_ID`, where `OWNER_TYPE` is a one-character code for the type ("D" - deal, "L" - lead), and `OWNER_ID` is the identifier.

## Method Parameters

See the description of [list methods](../../../how-to-call-rest-api/list-methods-pecularities.md)

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"OWNER_TYPE":"D","OWNER_ID":"1"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.productrow.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"OWNER_TYPE":"D","OWNER_ID":"1"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.productrow.list
    ```

- JS

    ```js
    var ownerType = prompt("Enter the owner type (D, L)");
    var ownerId = prompt("Enter the owner ID");
    BX24.callMethod(
        "crm.productrow.list",
        {
            filter:
            {
                "OWNER_TYPE": ownerType,
                "OWNER_ID": ownerId
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

    $ownerType = 'D'; // Replace 'D' with the desired owner type
    $ownerId = 1; // Replace 1 with the actual owner ID

    $result = CRest::call(
        'crm.productrow.list',
        [
            'filter' =>
            [
                'OWNER_TYPE' => $ownerType,
                'OWNER_ID' => $ownerId
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}