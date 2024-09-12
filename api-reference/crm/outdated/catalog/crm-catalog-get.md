# Get Product Catalog by ID crm.catalog.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the product catalog by ID.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md)| Identifier of the product catalog ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_id_here"}' \ # Replace 'your_id_here' with the actual ID
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.catalog.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_id_here"}' \ # Replace 'your_id_here' with the actual ID
    https://**put_your_bitrix24_address**/rest/crm.catalog.get?auth=**put_access_token_here**
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.catalog.get",
        { id: id },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $id = 'your_id_here'; // Replace 'your_id_here' with the actual ID

    $result = CRest::call(
        'crm.catalog.get',
        ['id' => $id]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}