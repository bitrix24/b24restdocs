# Get a List of Sections crm.productsection.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.productsection.list` returns a list of product sections based on a filter. It is an implementation of a list method for product sections. It is expected that the filter will include the `CATALOG_ID` parameter. Otherwise, sections will be selected from the default catalog.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | The array contains a list of fields to be selected ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected items ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the selected items  ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    catalogId=$(prompt "Enter the catalog ID")

    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "order": { "NAME": "ASC" },
        "filter": { "CATALOG_ID": '"$catalogId"' },
        "select": [ "ID", "NAME" ]
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.productsection.list
    ```

- cURL (OAuth)

    ```curl
    catalogId=$(prompt "Enter the catalog ID")

    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "order": { "NAME": "ASC" },
        "filter": { "CATALOG_ID": '"$catalogId"' },
        "select": [ "ID", "NAME" ],
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/crm.productsection.list
    ```

- JS

    ```js
    var catalogId = prompt("Enter the catalog ID");
    BX24.callMethod(
        "crm.productsection.list",
        {
            order: { "NAME": "ASC" },
            filter: { "CATALOG_ID": catalogId },
            select: [ "ID", "NAME" ]
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

    $catalogId = readline("Enter the catalog ID: ");

    $result = CRest::call(
        'crm.productsection.list',
        [
            'order' => [ 'NAME' => 'ASC' ],
            'filter' => [ 'CATALOG_ID' => $catalogId ],
            'select' => [ 'ID', 'NAME' ]
        ]
    );

    if (isset($result['error'])) {
        echo "Error: " . $result['error_description'] . "\n";
    } else {
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ```

{% endlist %}